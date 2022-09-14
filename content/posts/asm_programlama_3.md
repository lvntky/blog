---
author:
  name: Levent Kaya
date: 2022-08-14
linktitle: x86_64 Islemcilerde Assembly Programlama - 3 - Ilk Program 
type:
- post
- posts
title: x86_64 Islemcilerde Assembly Programlama - 3 - Ilk Program
weight: 10
series: 
---
# BOLUM 3 - ILK PROGRAM

Merhaba, serinin bu bolumunde ilk assembly programimizi yazacagiz. Bu bolumde yazdiklariniz size ilk basta yabanci gelebilir ve gozunuzu korkutabilir ama bu hic onemli degil. Ilerleyen surecte her seyin nasil isledigi oturacaktir. 

## Programa Giris

Ilk programimiz oldukca basit, o kadar basit ki programimiz cikis yapmak disinda hicbir sey yapmiyor. Bu bolumde yazdiklarinizi anlamak icin ugrasmayin bu bolumun tek gayesi sizi assembly programlamaya alistirmak ve bir linux makinede nasil calistirildigini gostermek. Bu postun devaminda programin nasil calistigini detaylandiracak ve izah edecegiz, ama simdi yazalim. 

```
#PURPOSE: Simple program that exists and return a
#         status code back to the linux kernel
#

#Input: None
#
#

#Outpur: Returns a status code
#        It can be viewed by typing
#
#        echo $?
#

#VARIABLES:
#         %eax, holds the system call number
#         %ebx, holds the return status
#

.section    .data
.section    .text
.globl _start
_start:
movl $1, %eax    # this is the linux kernel command
                 # number (system call) for exiting 
				 # a program
				 
movl $0, %ebx    # this is the status number we will 
                 # return to the operating system
				 
				 
int $0x80        # wakes up kernel to run 
	             # the exit command.
```
