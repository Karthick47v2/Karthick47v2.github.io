---
layout: default
title: Parallel Matrix Multiplication
---

Matrix multiplication is a fundamental operation in linear algebra. It is a crucial operation in many mathematical and scientific fields, including computer graphics, machine learning, and engineering. However, this can be computationally expensive and time-consuming, especially when the matrices are large. We can speed it up by sharing the work among multiple cores.

In this article, I have used OpenMP to parallelize matrix multiplication. OpenMP (Open Multiprocessing) is a popular open-source library for parallel programming in C, C++ and FORTRAN. We are going to analyze the performance of matrix multiplication for squared matrices with dimensions ranging from 100 to 1000 assuming that, you already know what matrix multiplication is.

**NOTE:** As we are only focusing about matrix multiplication, I leave matrix initialization and printing parts up to you. But you can always see the complete implementation [here](https://github.com/Karthick47v2/parallel-matrix-multiplication).

## Sequential Approach

First let’s see naive matrix multiplication implementation where each operation run sequentially.

```
for (short int i = 0; i < MAT_DIMS; i++)
    {
        for (short int j = 0; j < MAT_DIMS; j++)
        {
            for (short int k = 0; k < MAT_DIMS; k++)
            {
                mat[i * MAT_DIMS + j] += matA[i * MAT_DIMS + k] * matB[k * MAT_DIMS + j];
            }
        }
    }
```

The first loop used to iterate over rows of Matrix A and second loop used to iterate over columns of Matrix B. As we know, to get Matrix(i, j) we need to multiply iᵗʰ row of Matrix A with jᵗʰ column of Matrix B. In order to multiply each element we need another loop, which is the 3rd loop in above implementation. The time complexity of this approach is O(n³) which means that executing time grows at a rate of n³ as the size of the matrices increases. Note that I have used 1D array instead of 2D array for matrices as it is likely to be faster since it offers better memory locality, less allocation and deallocation overhead, and it consumes less memory than 2D dynamic arrays.

There are some algorithms which can be used to optimize and reduce time complexity in sequential manner. For example, Strassen’s matrix multiplication algorithm, faster than naive approach. However, these algorithms can be more complex and may not be always be practical to use in all situations.

## Parallel Approach

To parallelize this operation using OpenMP, we can use the **#pragma omp parallel** directive, which tells the compiler to parallelize the code segment enclosed by it.

```
#pragma omp parallel shared(matA, matB, mat)
    {
        short int numThreads = omp_get_num_threads();
        short int threadNo = omp_get_thread_num();

        for (short int i = threadNo; i < MAT_DIMS; i += numThreads)
        {
            for (short int j = 0; j < MAT_DIMS; j++)
            {
                for (short int k = 0; k < MAT_DIMS; k++)
                {
                    mat[i * MAT_DIMS + j] += matA[i * MAT_DIMS + k] * matB[k * MAT_DIMS + j];
                }
            }
        }
    }
```

The **shared()** clause specifies that the matrices should be shared between threads. **omp_get_num_threads()** will return number of threads participating in parallel execution, and **omp_get_thread_num()** will return thread ID of current thread. Here, we are parallelizing outer loop only, you can see that starting row ID differs for each thread and using **numThreads** to increment loop variable **i** ensures that each thread will access unique rows of Matrix A. Also, we don’t need to consider race condition as each thread updates different memory location in Resultant Matrix.

Actually, we don’t need to manually divide range for each thread. OpenMP will do all these things by itself for loops if we use **#pragma omp parallel for** directive. Here is an example of how to parallelize matrix multiplication using OpenMP way.

```
#pragma omp parallel for shared(matA, matB, mat)
        for (short int i = 0; i < MAT_DIMS; i++)
        {
            for (short int j = 0; j < MAT_DIMS; j++)
            {
                for (short int k = 0; k < MAT_DIMS; k++)
                {
                    mat[i * MAT_DIMS + j] += matA[i * MAT_DIMS + k] * matB[k * MAT_DIMS + j];
                }
            }
        }
```

By using OpenMP to parallelize the matrix multiplication operation, we can take advantage of multiple cores to speed up the computation. This can be especially useful for large matrices, where the computational cost of matrix multiplication can be quite high. But still, we can optimize it further. The major problem with the above approach is, this implementation doesn’t favor cache.

## Optimized Parallel Approach (Utilizing Row Major)

When a process requires some data from memory, usually its adjacent data also copied to cache for faster access. Usually, arrays in C are row major, which means row elements are stored next to each other in memory. So, if we fetch an array element from memory, its adjacent elements will get stored in cache. We are utilizing this for first loop (iterating over row), but not for second loop. Because for each row, we are only using 1 element (multiplying Matrix A’s row by Matrix B’s column). Therefore, we are wasting cached adjacent elements of Matrix B for each column (adding overhead of accessing them directly later). So, in the 3rd approach, we are going to utilize properties of C array.

```
#pragma omp parallel shared(matA, matB, mat)
    {
        short int numThreads = omp_get_num_threads();
        short int threadNo = omp_get_thread_num();

        unsigned short int temp[MAT_DIMS];

        for (short int i = threadNo; i < MAT_DIMS; i += numThreads)
        {
            memset(temp, 0, MAT_DIMS * sizeof(short int));
            for (short int j = 0; j < MAT_DIMS; j++)
            {
                for (short int k = 0; k < MAT_DIMS; k++)
                {
                    temp[k] += matA[i * MAT_DIMS + j] * matB[j * MAT_DIMS + k];
                }
            }
            memcpy(&mat[i * MAT_DIMS], temp, MAT_DIMS * sizeof(short int));
        }
    }
```

In the above implementation, instead of directly finding each resultant elements, we are finding all elements in each row of Resultant Matrix and updating it. Below shown image will give some insights about what is happening.

<<<<<<<<PIC>>>>>>>>
