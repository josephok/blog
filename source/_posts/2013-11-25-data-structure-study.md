---
layout: post
title: '数据结构学习'
date: 2013-11-25 05:39
comments: true
categories: 数据结构与算法
---
## 一些数学知识：

$$
x^ax^b = x^{a+b}
$$
$$
\frac{x^a}{x^b} = x^{a-b}
$$
$$
(x^a)^b = x^{ab}
$$
$$
x^n + x^n = 2x^n \ne x^{2n}
$$
$$
2^n + 2^n = 2^{n+1}
$$
$$
x^a = b => \log_x b = a
$$
$$
\log_a b = \frac{\log_c b} {\log_c a}
$$
$$
\log {ab}= \log a+ \log b
$$
$$
\log a/b = \log a- \log b
$$
$$
\log a^b = b\log a
$$
$$
\log x < x  (x > 0)
$$
$$
 \sum_{i=0}^n 2^i = 2^{n + 1} -1
$$
$$
\sum_{i=0}^n a^i = \frac{a^{n+1}-1} {a-1}
$$
$$
\sum_{i=1}^N i^2 = \frac{N(N+1)(2N+1)} 6 \approx \frac {N^3} 3
$$
$$
\sum_{i=1}^N i^k \approx \frac {N^{k+1}} {| {k+1} | }(k\ne-1)
$$

* 二叉树

二叉树的每个结点至多只有二棵子树(不存在度大于2的结点)，二叉树的子树有左右之分，次序不能颠倒。二叉树的第i层至多有
$$
2^{i-1}
$$
个结点；深度为k的二叉树至多有
$$
2^{k-1}
$$
个结点；对任何一棵二叉树T，如果其终端结点数为$n_0$，度为2的结点数为$n_2$，则
$$
n_0=n_2+1
$$

* 完全二叉树和满二叉树

1. 满二叉树：一棵深度为k，且有2^k-1个节点成为满二叉树

2. 完全二叉树：深度为k，有n个节点的二叉树，当且仅当其每一个节点都与深度为k的满二叉树中序号为1至n的节点对应时，称之为完全二叉树

* 二叉查找树

二叉查找树（Binary Search Tree），也称有序二叉树（ordered binary tree）,排序二叉树（sorted binary tree），是指一棵空树或者具有下列性质的二叉树：

1. 若任意节点的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
2. 任意节点的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
3. 任意节点的左、右子树也分别为二叉查找树。
4. 没有键值相等的节点（no duplicate nodes）。

二叉查找树相比于其他数据结构的优势在于查找、插入的时间复杂度较低。为O(log n)。

* AVL树

AVL树是最先发明的自平衡二叉查找树。在AVL树中任何节点的两个子树的高度最大差别为一，所以它也被称为高度平衡树。
查找、插入和删除在平均和最坏情况下都是O（log n）。增加和删除可能需要通过一次或多次树旋转来重新平衡这个树。
AVL树得名于它的发明者G.M. Adelson-Velsky和E.M. Landis，他们在1962年的论文《An algorithm for the organization of information》中发表了它。

节点的平衡因子是它的左子树的高度减去它的右子树的高度（有时相反）。带有平衡因子1、0或 -1的节点被认为是平衡的。带有平衡因子 -2或2的节点被认为是不平衡的，并需要重新平衡这个树。平衡因子可以直接存储在每个节点中，或从可能存储在节点中的子树高度计算出来。

AVL平衡二叉树的实现：

```c
avl.h:

#ifndef _AVL_H
#define _AVL_H
typedef struct node *Position;
typedef Position AVLtree;

typedef struct node
{
	int height;
	AVLtree left;
	AVLtree right;
	int data;
}node;

int Height(Position p); //return the height of a node
Position SingleRotateWithRight(Position k2); //left rotate when in RR operation
Position SingleRotateWithLeft(Position k2); // LL
Position DoubleRotateWithLeft(Position k3); // LR
Position DoubleRotateWithRight(Position k3); // RL
Position Rotate(Position k);
AVLtree insert(AVLtree root, int data); //Insert operation
Position search(AVLtree root, int data);
AVLtree delete(AVLtree root, int data);
void inorder_print(AVLtree root);
void dispose(AVLtree root); //free the tree
#endif

avl.c:

#define MAX(a, b) ((a) > (b) ? (a) : (b))
#include "avl.h"
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char const *argv[])
{
	AVLtree root = NULL;
	char choice;
	printf("Welcome to the AVL tree program\n");
	while (1) {
		printf("Please make a choice\n");
		printf("----------------------\n");
		printf("i. insert an element\n");
		printf("p. print the tree\n");
		printf("d. delete an element from the tree\n");
		printf("f. find an element \n");
		printf("q. quit the program\n");
		printf("----------------------\n");

		/*get rid of '\n' in buffer*/
		if ((choice = getchar()) == '\n')
			choice = getchar();

		int number;
		Position p;
		switch (choice) {
			case 'i':
				printf("Which number to insert?\n");
				scanf("%d", &number);
				root = insert(root, number);
				break;
			case 'p':
				printf("The tree is \n");
				printf("######################\n");
				inorder_print(root);
				printf("######################\n");
				break;
			case 'd':
				printf("Which number to delete?\n");
				scanf("%d", &number);
				root = delete(root, number);
				break;
			case 'f':
				printf("Which number to find?\n");
				scanf("%d", &number);
				if (p = search(root, number)) {
					printf("%d is found\n", number);
				}
				else
					printf("Not found\n");
				break;
			case 'q':
				printf("Byebye!\n");
				dispose(root);
				goto QUIT;
			default:
				printf("Wrong choice! Please try again\n");
				break;
		}
	}
	QUIT: return 0;
}

int Height(Position p) {
	if (p == NULL)
		return -1;
	else
		return p->height;
}

Position SingleRotateWithRight(Position k2) {
	Position k1 = k2->right;
	k2->right = k1->left;
	k1->left = k2;

	k1->height = MAX(Height(k1->left), Height(k1->right)) + 1;
	k2->height = MAX(Height(k2->left), Height(k2->right)) + 1;
	return k1;
}

Position SingleRotateWithLeft(Position k2) {
	Position k1 = k2->left;
	k2->left = k1->right;
	k1->right = k2;

	k2->height = MAX(Height(k2->left), Height(k2->right)) + 1;
	k1->height = MAX(Height(k1->left), Height(k1->right)) + 1;
	return k1;
}

Position DoubleRotateWithLeft(Position k3) {
	k3->left = SingleRotateWithRight(k3->left);
	return SingleRotateWithLeft(k3);
}

Position DoubleRotateWithRight(Position k3) {
	k3->right = SingleRotateWithLeft(k3->right);

	return SingleRotateWithRight(k3);
}

Position Rotate(Position k) {
	if (Height(k->left) - Height(k->right) == 2) {
		if (Height(k->left->left) >= Height(k->left->right)) {
			k = SingleRotateWithLeft(k);
		}
		else {
			k = DoubleRotateWithLeft(k);
		}
	}

	if (Height(k->right) - Height(k->left) == 2) {
		if (Height(k->right->right) >= Height(k->right->left)) {
			k = SingleRotateWithRight(k);
		}
		else {
			k = DoubleRotateWithRight(k);
		}
	}

	return k;
}

AVLtree insert(AVLtree root, int data) {
	if (root == NULL) {
		/*if the tree is empty then create a node*/
		root = (node*)malloc(sizeof(node));
		root->left = root->right = NULL;
		root->data = data;
		root->height = 0;
	}

	else if (data < root->data) {
		root->left = insert(root->left, data);
		if (Height(root->left) - Height(root->right) == 2) {
			if (data < root->left->data) {
				root = SingleRotateWithLeft(root);
			}
			else {
				root = DoubleRotateWithLeft(root);
			}
		}
	}
	else if (data > root->data) {
		root->right = insert(root->right, data);
		if (Height(root->right) - Height(root->left) == 2) {
			if (data > root->right->data) {
				root = SingleRotateWithRight(root);
			}
			else {
				root = DoubleRotateWithRight(root);
			}
		}
	}
	else {
		/*data already exsits*/
		fprintf(stderr, "%d exsits\n", data);
	}

	root->height = MAX(Height(root->left), Height(root->right)) + 1;

	return root;
}

Position search(AVLtree root, int data) {
	if (root == NULL) {
		return root;
	}

	if (root->data == data)
		return root;
	else if (root->data < data) {
		return search(root->right, data);
	}
	else {
		return search(root->left, data);
	}
}

AVLtree delete(AVLtree root, int data) {
	if (root == NULL) {
		fprintf(stderr, "Not found\n");
		return NULL;
	}

	else if (data == root->data) {
		if (root->right == NULL) {
			/*if right child is NULL then delete it*/
			Position temp = root;
			root = root->left;
			free(temp);
			printf("%d is deleted\n", data);
		}
		else {
			Position temp = root->right;
			while (temp->left) {
				temp = temp->left;
			}

			root->data = temp->data;
			root->right = delete(root->right, temp->data);
			root->height = MAX(Height(root->left), Height(root->right)) + 1;
			printf("%d is deleted\n", data);
		}

		return root;
	}

	else if (data < root->data) {
		root->left = delete(root->left, data);
	}
	else {
		root->right = delete(root->right, data);
	}

	if (root->left) {
		root->left = Rotate(root->left);
	}
	if (root->right) {
		root->right = Rotate(root->right);
	}

	root = Rotate(root);
	root->height = MAX(Height(root->left), Height(root->right)) + 1;

	return root;
}

void inorder_print(AVLtree root) {
	if (root != NULL) {
		inorder_print(root->left);
		printf("%d\n", root->data);
		inorder_print(root->right);
	}
}

void dispose(AVLtree root)
{
    if( root != NULL )
    {
        dispose( root->left );
        dispose( root->right );
        free( root );
    }
}
```

* B树

A B-tree of order m is an m-way tree (i.e., a tree where each node may have up to m children) in which:

1.	the number of keys in each non-leaf node is one less than the number of its children and these
keys partition the keys in the children in the fashion of a search tree
2.	all leaves are on the same level
3.	all non-leaf nodes except the root have at least ceil(m / 2) children
4.	the root is either a leaf node, or it has from two to m children
5.	a leaf node contains no more than m – 1 keys

* Hash表

散列表（Hash table，也叫哈希表），是根据关键字（Key value）而直接访问在内存存储位置的数据结构。也就是说，它通过把键值通过一个函数的计算，映射到表中一个位置来访问记录，这加快了查找速度。这个映射函数称做散列函数，存放记录的数组称做散列表。

* Hash表处理碰撞

为了知道碰撞产生的相同散列函数地址所对应的关键字，必须选用另外的散列函数，或者对碰撞结果进行处理。而不发生碰撞的可能性是非常之小的，所以通常对碰撞进行处理。常用方法有以下几种：

- 开放寻址法（open addressing）：

$$
hash_i=(hash(key)+d_i) \,\bmod\, m, i=1,2...k\,(k \le m-1)
$$

其中hash(key)为散列函数，m为散列表长，
$d_i$为增量序列，$i$为已发生碰撞的次数。增量序列可有下列取法：

$$
d_i=1,2,3...(m-1)
$$

称为线性探测；即 $d_i=i$ ，或者为其他线性函数。
相当于逐个探测存放地址的表，直到查找到一个空单元，把散列地址存放在该空单元。

$$
d_i=\pm 1^2, \pm 2^2,\pm 3^2...\pm k^2 (k \le m/2)
$$
称为平方探测。相对线性探测，
相当于发生碰撞时探测间隔 $d_i=i^2$ 个单元的位置是否为空，如果为空，将地址存放进去。

$d_i=$伪随机数序列，称为`伪随机探测`。


* Hash表载荷因子

散列表的载荷因子定义为：
$\alpha = $填入表中的元素个数 / 散列表的长度。$\alpha$是散列表装满程度的标志因子。


由于表长是定值，载荷因子与“填入表中的元素个数”成正比，所以，载荷因子越大，表明填入表中的元素越多，产生冲突的可能性就越大；反之，载荷因子越小，标明填入表中的元素越少，产生冲突的可能性就越小。实际上，散列表的平均查找长度是载荷因子载荷因子的函数，只是不同处理冲突的方法有不同的函数。

对于开放寻址法，荷载因子是特别重要因素，应严格限制在0.7-0.8以下。超过0.8，查表时的CPU缓存不命中（cache missing）按照指数曲线上升。因此，一些采用开放寻址法的hash库，如Java的系统库限制了荷载因子为0.75，超过此值将resize散列表。

To be continued...