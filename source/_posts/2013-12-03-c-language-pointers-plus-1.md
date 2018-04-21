---
layout: post
title: 'c语言指针加1'
date: 2013-12-03 07:57
comments: true
categories: 编程语言
tags: C
---
C语言中，指针变量存储的是地址，那么我们不禁要问：将指针变量加1，那么地址变为多少？
比如某个指针变量p的值为`0x8989f580`，p + 1的值是多少？

这就要看指针指向的变量类型了，假设int型占32位（4个字节），p是指向int型的指针，p + 1实际上是加4；如果是char型，那么p + 1实际上是加1（因为char型占1个字节）。所以总结为：p + 1 == p + sizeof(p指向的类型)。

这里还有考虑一个特殊情况，那就是`void*`指针，如果p是`void *`指针，那么p + 1还是将地址值加1，而`sizeof(void *)`却是4。下面例子可以说明：

```c
int main(int argc, char const *argv[])
{
	void *p;
	printf("%x\n", p);
	printf("%x\n", p + 1);
	printf("%d\n", sizeof(void *));
}
```

输出结果为：

895f0230
895f0231
4

我们知道`malloc()`和`calloc()`函数返回的都是`void*`指针，将其赋值给某个指针变量时会自动进行类型转换，所以不会出现指针不匹配的问题。