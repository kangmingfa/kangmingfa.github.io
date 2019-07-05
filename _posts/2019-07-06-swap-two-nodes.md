---
layout: post
title: "交换链表任意节点"
author: "fmitk"
categories: sample
tags: [sample]
image: blog_003.jpeg
---
# 交换链表任意节点

```C
#include <stdio.h>
#include <stdlib.h>
#define N 10
typedef struct node
{
    int data;
    struct node* next;
}NODE;

void CreateLinkedList(NODE**head,int arr[])
{
    NODE* track;
    for(int i = 0;i<N;i++)
    {

        NODE* new_node = (NODE*)malloc(sizeof(NODE));
        new_node->data = arr[i];
        //这里采用尾插法
        if(*head==NULL)
        {
            *head = new_node;
            new_node->next = NULL;
        }
        else
        {
            track->next = new_node;
            new_node->next = NULL;
        }
        track = new_node;

    }
}
void SwapTwoNodes(NODE** head,NODE* prev_x,NODE* prev_y,NODE* node_x,NODE* node_y)
{
    NODE* temp;
    prev_x->next = node_y;
    prev_y->next = node_x;
    temp = node_y->next;
    node_y->next = node_x->next;
    node_x->next = temp;
}
void SearchTwoNodes(NODE** head,int x,int y)
{
    if(x==y)
    {
        printf("\n值相等不进行交换\n");
        return NULL;
    }
    NODE* p = *head;
    NODE* node_x,*node_y,*pre;
    NODE* prev_x,*prev_y;
    NODE *temp;
    while(p!=NULL)
    {
        if(p->data==x)
        {
            prev_x = pre;
            node_x = p;
        }
        else if(p->data==y)
        {
            prev_y = pre;
            node_y = p;
        }
        pre = p;
        p = p->next;

    }
    //判断要交换的两个节点其中一个是否有一个为头节点
    if(node_x->data==(*head)->data)
    {
        *head = node_y;
        prev_y->next = node_x;
        temp = node_x->next;
        node_x->next = node_y->next;
        node_y->next = temp;

    }
    else if(node_y->data==(*head)->data)
    {
        *head = node_x;
        prev_x->next = node_y;
        temp = node_y->next;
        node_y->next = node_x->next;
        node_x->next = temp;
    }
    else
        SwapTwoNodes(&*head,prev_x,prev_y,node_x,node_y);
}

void disLinkedList(NODE* head)
{
    NODE* p = head;
    while(p!=NULL)
    {
        if(p->next!=NULL)
        {
            printf("%d-->",p->data);
        }
        else
        {
            printf("%d",p->data);
        }

        p = p->next;
    }
    printf("\n");
}


int main()
{
    srand(3);
    int arr[N];
    NODE* head = NULL;

    for(int i = 0;i<N;i++)
    {
        arr[i] = rand()%100;
    }
    CreateLinkedList(&head,arr);
    printf("交换前 : ");
    disLinkedList(head);
    SearchTwoNodes(&head,arr[1],arr[1]);
    printf("\n");
    printf("交换后 : ");
    disLinkedList(head);
//    for(int i = 0;i<N;i++)
//    {
//        printf("%d\t",arr[i]);
//    }
    return 0;
}

```