---
title: 数据结构与算法「线性表c」
date: 2018-01-29 21:25:15
tags: 代码区块
---

```objc
 * 结构代码
#define LIST_INIT_SIZE 20	//存储空间初始分配量
#define LIST_INCREMENT 2	//存储空间分配增量
typedef int ElemType;		//根据实际类型而定，假设为int
typedef struct {
	ElemType *elem;			//数据存储空间基址，或者(ElemType elem[LIST_INIT_SIZE])
	int size;				//当前数据存储空间容量
	int len;				//当前数据长度(元素个数)
} SQList;					//顺序存储结构的结构体定义

* 构造空表
	- 1. 分配初始存储空间
	- 2. 如果分配失败，抛出异常
	- 3. 初始化当前数据存储容量和长度
InitSequenceList(SQList *list) {
	list->elem = malloc(sizeof(ElemType) * LIST_INIT_SIZE);
	if (!list->elem) {
		exit(0);
	}
	list->size = LIST_INIT_SIZE;
	list->len = 0;
}

* 获得元素
	- 1. 如果查找的位置不合理，抛出异常
	- 2. 根据基址及查找位置，通过地址偏移获得元素
GetSequenceListElement(SQList *list, int index, ElemType *e) {
	if (index <= 0 || index > list->len) {
		exit(-1);
	}
	if (0 == list.len) {
		exit(0);
	}
	*e = *(list->elem + (index-1));
}

* 插入元素
	- 1. 如果插入的位置不合理，抛出异常
	- 2. 如果当前存储空间容量不足，则抛出异常或动态增加存储容量
	- 3. 从最后一个元素开始向前遍历至第i个位置，并将其间元素都向后移动一个位置
	- 4. 将元素插入位置i处，并将表长度+1
InsertSequenceListElement(SQList *list, int index, ElemType elem) {
	if (index <= 0 || index > list->len) {
		exit(-1);
	}
	if (list->len == list->size) {
		//exit(0);
		ElemType *n_elem = remalloc(list->elem, sizeof(ElemType) * (list->size+LIST_INCREMENT));
		if (!n_elem) {
			exit(0);
		}
		list->elem = n_elem;
		list->size += LIST_INCREMENT;
	}
	ElemType *loc = list->elem + (index-1);
	ElemType *tail = list->elem + list->len;
	for (; loc <= tail; loc++) {
		*(loc+1) = *loc;
	}
	*loc = e;
	list->len += 1;
}

* 删除元素
	- 1. 如果删除的位置不合理，抛出异常
	- 2. 从第i个位置开始向后遍历至最后一个元素，并将其间元素都向前移动一个位置
	- 3. 将表长度-1
DeleteSequenceListElement(SQList *list, int index, ElemType *e) {
	if (index <= 0 || index > list->len) {
		exit(-1);
	}
	ElemType *loc = list->elem + (index-1);
	ElemType *tail = list->elem + list->len;
	*e = *loc;
	for (; loc < tail; loc++) {
		*loc = *(loc+1);
	}
	list->len -= 1;
}

* 查找元素
	- 1. 从第1个位置开始向后遍历元素，当找到元素时则退出
	- 2. 如果遍历过线性表长度或元素不存在，抛出异常
LocateSequenceListElement(SQList *list, ElemType elem) {
	int i = 0;
	while (*(list->elem+i) != elem && i < list->len) {
		i++;
	}
	if (i >= list->len) {
		return -1;
	}
	return i+1;
}

* 表的长度
 	- 1. 线性表结构的当前数据长度
 SequenceListLength(SQList *list) {
 	return list->len;
 }


* 表的判空
	- 1. 线性表长度是否为0，则表示表空
IsSequenceListEmpty(SQList *list) {
	return (0 == list->len);
}

* 表的置空
	- 1. 线性表长度置为0，则表示表空
ClearSequenceList(SQList *list) {
	list->len = 0;
}

* 表的销毁
	- 1. 释放结构体所占内存空间
	- 2. 置空结构体初始化变量
DestroySequenceList(SQList *list) {
	free(list->elem);
	list->elem = NULL;
	list->size = 0;
	list->len = 0;
}
```