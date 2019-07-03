---
layout: post
title: "反转链表"
author: "fmitk"
categories: sample
tags: [sample]
image: blog_001.jpeg
---
# 反转链表

![img](/assets/post_imgs/reverselist.gif)

```C
#include <stdio.h>
#include <stdlib.h>
#define N 5
typedef struct node
{
    int data;
    struct node* next;
}NODE;

void CreateLinkedlist(NODE** head_ref)
{
    //head = (struct node*)malloc(sizeof(NODE));
    NODE* new_node,temp;
    for(int i = 0; i < N;i++)
    {
        new_node = (NODE*)malloc(sizeof(NODE));
        new_node->data = i;
        new_node->next = *head_ref;
        *head_ref =  new_node;
    }
}
void reverselinkedlist(NODE** head_ref)
{
    NODE* curr = *head_ref;
    NODE* prev = NULL;
    NODE* next = NULL;
    while(curr!=NULL)
    {
        next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    *head_ref = prev;
}
void displinkedlist(NODE* head)
{
    NODE *p;
    p = head;
    int data;
    if(head->next==NULL)
    {
        printf("No data\n");
    }
    while(p!=NULL)
    {
        data = p->data;
        if(p->next!=NULL)
        {
           printf("%d-->",data);
        }
        else
        {
            printf("%d",data);
        }
        p = p->next;
    }
    printf("\n");
}

int main()
{
    NODE* head = NULL;
    CreateLinkedlist(&head);
    printf("\n初始链表 : \n");
    displinkedlist(head);
    reverselinkedlist(&head);
    printf("\n反转后链表 : \n");
    displinkedlist(head);
    return 0;
}
```