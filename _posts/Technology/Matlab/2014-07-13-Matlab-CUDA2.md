---
layout: post
title: Matlab中的CUDA环境配置小记
categories: 技术
tags: Matlab CUDA
---

>参考书籍《Accelerating MATLAB with GPU Computing》—Chapter 2.4

首先根据系统情况下载CUDA安装包并安装。

安装完毕后再Matlab中输入：
    >> system('nvcc');
    
正常情况下会返回值:

    nvcc : fatal error : No input files specified; use option --help for more information 
    
但这是正常现象，如果返回值为：

    nvcc is not recognized as an 26 Accelerating MATLAB with GPU Computing internal or external command, operable program or batch file
    
则可能是由于CUDA没有正确安装

根据教程，接着在Matlab中新建一个**AddVectors.h**文件：

``` matlab
#ifndef __ADDVECTORS_H__
#define __ADDVECTORS_H__
extern void addVectors(float* A, float* B, float* C, int size);
#endif // __ADDVECTORS_H__
```

再建立一个**AddVectors.cu**文件

``` matlab
#include "AddVectors.h"
#include "D:\Program Files\MATLAB\R2012b\extern\include\mex.h"


__global__ void addVectorsMask(float*A, float*B, float*C, int size)
{
    int i=blockIdx.x;
    if(i>= size)
        return;
    C[i] = A[i] + B[i];
}

void addVectors(float* A, float* B, float* C, int size)
{
    float *devPtrA = 0,*devPtrB = 0,*devPtrC = 0;
    cudaMalloc(&devPtrA, sizeof(float) * size);
    cudaMalloc(&devPtrA, sizeof(float) * size);
    cudaMalloc(&devPtrA, sizeof(float) * size);

    cudaMemcpy(devPtrA,A, sizeof(float)* size, cudaMemcpyHostToDevice);
    cudaMemcpy(devPtrB,B, sizeof(float)* size, cudaMemcpyHostToDevice);
    addVectorsMask<<<size, 1>>>(devPtrA,devPtrB, devPtrC, size);
    cudaMemcpy(C,devPtrC, sizeof(float)* size, cudaMemcpyDeviceToHost);

    cudaFree(devPtrA);
    cudaFree(devPtrB);
    cudaFree(devPtrC);

}
```

根据教程，下一步为执行：

    >>system('nvcc -c AddVectors.cu')
    
但这时问题就出现了，Matlab可能会提示：

    nvcc : fatal error : Cannot find compiler 'cl.exe' in PATH 
    
这是Matlab在现有的路径中并没有找到VS 2010用来编译的cl.exe，本来想试着如何将VS的目录包含进去，但是试过在环境变量PATH中添加VS目录以用及Mex -setup连接VS环境后，错误均未解决，因此采用就书中提及的 **-ccbin** 选项（为了以后编程方便，建议将VS的bin目录和Matlab的\extern\include目录添加到环境变量PATH中）：

    >>system('nvcc -c AddVectors.cu -ccbin "C:\Program Files (x86)\Microsoft Visual Studio 10.0\VC\bin"')
    
于是就可以正确编译生成.obj文件了。
最开始编译的时候还遇到了无法打开“mex.h”的错误：

    fatal error C1083: 无法打开包括文件:“mex.h”: No such file or directory 
AddVectors.cu 

可以通过将刚刚编写的**AddVectors.cu**文件中的#include “mex.h”添加路径指向：

    #include "D:\Program Files\MATLAB\R2012b\extern\include\mex.h"
    
期间还遇到过无法打开“tmwtypes.h”的错误:

     fatal error C1083: 无法打开包括文件:“tmwtypes.h”: No such file or directory 
     
最终参考[木子超同学的博文](http://www.muzichao.com/matlab-cuda-2/)，将#include <tmwtypes.h>改为#include "tmwtypes.h"后通过编译：