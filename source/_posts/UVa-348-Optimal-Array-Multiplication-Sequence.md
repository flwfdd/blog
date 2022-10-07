---
title: '[UVa 348]Optimal Array Multiplication Sequence'
date: 2019-12-24 23:46:13
mathjax: true
categories: OI
tags:
- OI
- UVa
- dp
- 区间dp
---

[题目链接（洛谷）](https://www.luogu.com.cn/problem/UVA348)

哇哇哇UVa要翻墙好烦呐。
圣诞节咯！（虽然学校不允许庆祝，但是zou的糖很甜233）

这道题大抵是周六留下来的，拖了两天。

题目意思就是最小化矩阵乘法运算次数，给出一堆矩阵的长和宽，要你最小化乘法运算数量。

一看矩阵就已经准备好当场去世了，但是发现其实和矩阵的关系并没有那么大，比较容易地能想到区间dp。

区间dp标准三重循环搞上去，如果`l`、`r`分别为区间端点，`k`为区间断点，`t`表示行/列数，那么合并区间的时候的答案$f[l][r]=f[l][j]+f[j+1][r]+t[l] \times t[k] \times t[r]$

刚开始$t[k]$还泄漏了，还是得注意读题目，明明白白写了`a*b*c`呢。

[评测结果（洛谷）](https://www.luogu.com.cn/record/28739768)
```cpp
#include<bits/stdc++.h>

namespace fdd
{
	int T,n,f[20][20],t[20],out[20][2],ans[20][20];

	void printn(int l,int r)
	{
		if(l==r)return;
		++out[l][0];
		++out[r][1];
		printn(l,ans[l][r]);
		printn(ans[l][r]+1,r);
	}

	bool solve()
	{
		std::cin>>n;
		if(!n)return 0;
		for(int i=0;i<n;i++)scanf("%d%d",&t[i],&t[i+1]);
		memset(f,0x3f,sizeof(f));
		memset(out,0,sizeof(out));
		for(int i=1;i<=n;i++)f[i][i]=0;

		for(int k=1;k<n;k++)
		{
			for(int i=1;i<=n-k;i++)
			{
				for(int j=i;j<i+k;j++)
				{
					if(f[i][i+k]>f[i][j]+f[j+1][i+k]+t[i-1]*t[j]*t[i+k])
					{
						f[i][i+k]=f[i][j]+f[j+1][i+k]+t[i-1]*t[j]*t[i+k];
						ans[i][i+k]=j;
					}
					//printf("@%d %d %d\n",i,i+k,f[i][i+k]);
				}
			}
		}
		printn(1,n);
		printf("Case %d: ",++T);
		for(int i=1;i<=n;i++)
		{
			if(i>1)printf(" x ");
			while(out[i][0]--)printf("(");
			printf("A%d",i);
			while(out[i][1]--)printf(")");
		}
		printf("\n");


		return 1;
	}
}

int main()
{
	while(fdd::solve());
	return 0;
}
```