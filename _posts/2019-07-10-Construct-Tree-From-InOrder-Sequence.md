---
layout: post
title: "中序序列构造树二叉树"
author: "fmitk"
categories: sample
tags: [sample]
image: blog_004.jpg
---
# 在给定中序序列，结合先序，后序序列构造二叉树的两种方式

```C
#include <stdio.h>
#include <stdlib.h>
typedef struct node
{
    char value;
    struct node *left;
    struct node *right;
}NODE;
//定义先序序列和后序序列的起始下标
int preindex = 0;
int lateindex = 0;
int len;
NODE* CreateNode(char value)
{
    NODE *new_node = (NODE*)malloc(sizeof(NODE));
    new_node->value = value;
    new_node->left = NULL;
    new_node->right = NULL;
    return new_node;
}

int searchindex(int value,char in[])
{
    int i;
    for(i = 0;i<len;i++)
    {
        if(value==in[i])
            return i;
    }
}

NODE* buildtreefrompre(char pre[],char in[],int instart,int inend)
{
    if(instart > inend)
    {
        return NULL;
    }
    //遍历先序序列找到每个二叉树节点的左右子树
    char value = pre[preindex++];
    NODE *t_node = CreateNode(value);
    //递归出栈条件
    if(instart==inend)
        return t_node;
    //寻找当前节点在中序序列的索引，划分左右子树
    int index = searchindex(value,in);
    //时刻铭记每一个左右孩子节点又是一个二叉树，毕竟二叉树是用递归定义的
    t_node->left = buildtreefrompre(pre,in,instart,index - 1);
    t_node->right = buildtreefrompre(pre,in,index + 1,inend);
    return t_node;
}

NODE* buildtreefromlate(char late[],char in[],int instart,int inend)
{
    if(instart > inend)
        return NULL;
    char value = late[lateindex--];

    NODE *t_node = CreateNode(value);

    if(instart==inend)
        return t_node;
    int index = searchindex(value,in);
    t_node->right = buildtreefromlate(late,in,index+1,inend);
    t_node->left = buildtreefromlate(late,in,instart,index - 1);
    return t_node;
}
void dispalyTree(NODE *root)
{
    if(root)
    {
        dispalyTree(root->left);
        printf("%c\t",root->value);
        dispalyTree(root->right);
    }

}
int main()
{
    //利用中序序列构造树
    char pre[] = {'A','B','D','E','C','F'};
    char in[] = {'D','B','E','A','F','C'};
    char late[] = {'D','E','B','F','C','A'};
    len = sizeof(pre) / sizeof(pre[0]);
    lateindex = len - 1;
    NODE *root1 =  buildtreefrompre(pre,in,0,len - 1);
    NODE *root2 =  buildtreefromlate(late,in,0,len - 1);
    printf("\n======================中序序列和先序序列构造二叉树结果=======================\n\n");
    dispalyTree(root1);
    printf("\n\n======================中序序列和后序序列构造二叉树结果=======================\n\n");
    dispalyTree(root2);
    //printf("%d\n",searchindex('C',in));
    return 0;
}

```