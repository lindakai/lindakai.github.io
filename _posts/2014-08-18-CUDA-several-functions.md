---
layout: post
title:  "CUDA C 中的几个常用函数"
category: 技术
tags: CUDA LDPC
---

### 一、cudaMalloc()
    cudaMalloc(void** devPtr,  size_t cout);
devPtr: 在显存上分配数据的头指针

cout: 分配空间的大小，以字节为单位。
   
### 二、cudaMemcpy()
    cudaMemcpy(void* dst,  
               const void* src,  
               size_t count, 
               enum cudaMemcpyKind kind);
dst: 目的矩阵内存头指

src：源矩阵内存头指针

count: 拷贝数据的大小

kind：拷贝数据的方向
   
### 三、cudaMallocPitch()
    cudaMallocPitch(void** devPtr,  
                size_t* pitch,  
                size_t widthInBytes,  
                size_t height)
devPtr：开辟矩阵的数据的头指针

pitch：分配存储器的宽度，以字节为单位(cuda的返回值)

width：分配矩阵的列数

height：分配矩阵的行数
注意列空间的分配需要乘上元素的大小（eg. sizeof(float)）
cudaMallocPitch()来分配global memory时，对行空间的申请回扩大到128个Bytes的整数倍。从而使每行的首地址与globla memory分段地址对齐，warp在访问的时候也就是对齐的了。

### 四、cudaMemcpy2D()
    cudaMemcpy2D(void* dst,
                    size_t dpitch,
                    const void* src,
                    size_t spitch,
                    size_t width,
                    size_t height,
                    enum cudaMemcpyKind kind);
dst: 目的矩阵内存头指针

dpitch: dst指向的2D数组中的内存宽度，以字节为单位，是cuda为了读取方便，对齐过的内存宽度，可能大于一行元素占据的实际内存。

src：源矩阵内存头指针

spitch: src指向的2D数组中的内存宽度，以字节为单位

width: src指向的2D数组中一行元素占据的实际宽度。以字节为单位，等于
width*sizeof(type)

height: src指向的2D数组的行数。

kind：拷贝数据的方向

### 五、cudaMemset()

    cudaError_t cudaMemset (void * devPtr, 
                            int value, 
                            size_t count )
                            
devPtr：需要赋值矩阵的头指针

value：需要赋的值

count：需要赋值的空间长度

[查看原文](http://blog.sina.com.cn/s/blog_82a790120101ka1d.html)
