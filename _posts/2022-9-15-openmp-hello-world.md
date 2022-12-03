---
layout: post
title: "Write Hello World in OpenMP and C"
description: " How to write Hello World in OpenMP and C."
keywords: "OPENMP, C, Hello World, tutorial"
comments: true
---

-----------------------

Early this month i attened CoDaS hep summer school. It is meant for phycics graduate students to learn more about data analysis and machine learning in the field of high energy physics. During the school, we learned a lot of things about data analysis and machine learning. One of the topics we learned was how to use OpenMP to parallelize our code. In this post, I will show you how to write a simple Hello World program in OpenMP and C.

- What is OpenMP?

OpenMP is a standard for parallel programming in C, C++, and Fortran. It provides a set of directives, functions, and data environment that allows programmers to write parallel code that can be executed on multiple CPU cores.

OpenMP allows programmers to specify which parts of the code should be executed in parallel and which parts should be executed sequentially. This enables them to write efficient parallel programs that can take advantage of the multiple CPU cores in a system.

OpenMP also provides a set of tools and libraries that can be used to manage the execution of parallel code, such as scheduling the threads and controlling their synchronization. These tools and libraries can be used to improve the performance of parallel programs and make them more efficient.

OpenMP is widely used in many fields, including scientific computing, data analysis, and technical computing. It is supported by many compilers and platforms, and it is compatible with a wide range of programming languages.

- Why use C not C++?

Basically because it is what I have been introduced and C is a popular programming language that is widely used in many fields, including software development, scientific computing, and systems programming.

In this post, I will show you how to write a simple "Hello, World!" example in C and OpenMP. This example will demonstrate how to use the OpenMP library to parallelize a C program and execute it on multiple CPU cores.

To write a "Hello, World!" example in C and OpenMP, you first need to include the OpenMP header file in your program. This header file provides the declarations and definitions of the OpenMP functions and macros that you can use in your code.

To write a "Hello, World!" example in C and OpenMP, you first need to include the OpenMP header file in your program. This header file provides the declarations and definitions of the OpenMP functions and macros that you can use in your code.

``` c++
#include <omp.h>
```

Next, you need to define the `main()` function, which is the entry point of the program. In this function, you can use the `#pragma omp` parallel directive to specify that the code within the directive should be executed in parallel by multiple threads.

``` c++
int main() {
  #pragma omp parallel
  {
    // Code to be executed in parallel by multiple threads
  }
  return 0;
}
```

Within the `#pragma` omp parallel directive, you can use the `printf()` function to print the `"Hello, World!"` message. Now a more complete version of the "Hello, World!" example in C and OpenMP looks like this:

``` c++
#include <omp.h>
#include <stdio.h>
#include <stdlib.h>
int main (int argc, char *argv[])
{
    int nthreads, tid;
    #pragma omp parallel private(nthreads, tid)
    {
        tid = omp_get_thread_num();
        nthreads = omp_get_num_threads();
        printf("Hello World Thread %d / %d\n", tid, nthreads);
    }
    return 0;
}
```

This code is a simple "Hello, World!" example in C that uses the OpenMP library to parallelize the execution of the program. It includes the OpenMP header file `<omp.h>` and the standard input/output header file `<stdio.h>` to access the OpenMP functions and the `printf()` function, respectively.

In the `main()` function, the `#pragma omp` parallel directive as explained before is used to specify that the code within the directive should be executed in parallel by multiple threads. The `private()` clause is used to specify that the nthreads and tid variables should be private to each thread and not shared among them.

Within the `#pragma omp` parallel directive, the `omp_get_thread_num()` function is used to get the thread ID of the current thread, and the `omp_get_num_threads()` function is used to get the total number of threads. These values are then printed using the printf() function.

When this code is executed, the "Hello, World!" message will be printed multiple times, once for each thread that is executing the code within the `#pragma omp` parallel directive. The thread ID and the total number of threads will be printed with each message, so you can see how the code is being executed in parallel by multiple threads.

- How to compile and run the code?


1. Install GCC with OpenMP as default gcc in Mac is CLANG and doesn't have OMP.
2. Install gcc via home brew. `brew install gcc`
3. Maybe in your shell you can use `alias gcc=gcc-11` to make it easy (and follow online tutorials make)
4. To compile run `gcc -fopenmp omp.c -o omp`
5. Run the example by `./omp`

