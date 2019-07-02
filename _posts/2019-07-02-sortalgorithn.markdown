---
layout: post
title: "Sort Algorithm"
author: "fmitk"
categories: sample
tags: [sample]
image: cuba-1.jpg
---
# 冒泡-选择-插入排序

```C
#include <stdio.h>
#include <stdlib.h>
#include<time.h>
#define N 20
void swaptwonums(int* a,int* b)
{
    int temp;
    temp = *a;
    *a = *b;
    *b = temp;
}
void bubblesort(int* arr)
{
    int i,j;
    for(i=0;i<N-1;i++)
    {
        for(j=0;j<N-i-1;j++)
        {
            if(arr[j] > arr[j+1])
            {
                swaptwonums(&arr[j],&arr[j+1]);

            }
        }
    }
}
void arr_select(int* arr)
{
    //尽量减少交换次数
    int i,j;
    for(i = 0;i<N-1;i++)
    {
        int max_postion = i;
        for(j = i + 1;j < N;j++)
        {
            if(arr[max_postion] > arr[j])
            {
                max_postion = j;
                //swaptwonums(&arr[i],&arr[j]);
            }
        }
        if(max_postion!=i)
        {
            swaptwonums(&arr[max_postion],&arr[i]);
        }
    }
}
void insert_value(int* arr)
{
    int i,j,k;
    int temp;
    for(i = 1;i < N ;i++)
    {
        for(j = 0; j<i;j++)//寻找元素插入的位置
        {
            if(arr[j] > arr[i])
            {
                temp = arr[i];
                k = i - 1;
                while(k > j || k==j)//元素后移动
                {
                    arr[k + 1] = arr[k];
                    k--;
                }
                arr[j] = temp;

                break;
            }
        }
    }
}
void printarray(int* arr)
{

    for(int j = 0;j < N;j++)
    {
        printf("%d\t",arr[j]);
    }
    printf("\n");
}

int main()
{
    srand(3);//随机种子
    int arr[N];
    int i;
    for(i=0;i<N;i++)
    {
        arr[i] = rand()%100;
    }
    //printarray(arr);
    //bubblesort(arr);
    //arr_select(arr);
    insert_value(arr);
    printarray(arr);
    return 0;
}

```