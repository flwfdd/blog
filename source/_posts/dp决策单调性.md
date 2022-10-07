---
title: dp决策单调性
date: 2020-01-05 22:15:50
mathjax: true
categories: OI
tags:
- OI
- 决策单调性
- dp
- dp优化
---

# [IOI2000]邮局

[题目链接](https://www.luogu.com.cn/problem/P4767)

首先对输入排序。

记$f[i][j]$为从`1`到`i`个村庄中设立`j`个邮局的最小代价。

从左往右尝试求得答案。每次更新一个村庄，就需要找一个邮局来管这个村庄，那么枚举枚举一个断点`k`，`1`到`k`取`j-1`个邮局，其中具体摆放不关心，只关心代价。`k+1`到`i`取一个邮局，来管第i个村庄。

取一个村庄的就取中位数即可，初始化一下区间取一个的。
```cpp
#include<bits/stdc++.h>
namespace fdd
{
	int n,m,t[3030],f[3030][330],p[3030][330];
	
	int sum(int l,int r)
	{
		int mid=(l+r)>>1,mm=0;
		for(int i=l;i<=r;i++)mm+=std::abs(t[mid]-t[i]);
		return mm;
	}
	
	void solve()
	{
		int m1;
		std::cin>>n>>m;
		for(int i=1;i<=n;i++)scanf("%d",&t[i]);
		std::sort(t+1,t+1+n);
		
		for(int i=1;i<=n;i++)
		{
			f[i][1]=sum(1,i);
			m1=std::min(i,m);
			for(int j=2;j<=m1;j++)
			{
				f[i][j]=0x3f3f3f3f;
				for(int k=j-1;k<i;k++)
				{
					f[i][j]=std::min(f[i][j],f[k][j-1]+sum(k+1,i));
				}
				//printf("%d %d %d\n",i,j,f[i][j]);
			}
		}
		printf("%d",f[n][m]);
	}
}

int main()
{
	fdd::solve();
	return 0;
}
```

这样可以拿到40分。其他都`TLE`。

看了看题解能大概理解，但是明显还是没有完全理解四边形不等式，直观地能“显然”看出结论的正确性，但是要是从头来想，或是证明，就显得比较困难。

还是得硬着头皮看看四边形不等式的证明，这样才能从更本质的角度理解算法的核心。

```cpp
#include<bits/stdc++.h>
namespace fdd
{
	int n,m,t[3030],f[3030][330],p[3030][330],w[3030][3030];
	
	int sum(int l,int r)
	{
		int mid=(l+r)>>1,mm=0;
		for(int i=l;i<=r;i++)mm+=std::abs(t[mid]-t[i]);
		return mm;
	}
	
	void solve()
	{
		int m1,m2;
		std::cin>>n>>m;
		for(int i=1;i<=n;i++)scanf("%d",&t[i]);
		std::sort(t+1,t+1+n);
		
		for(int i=1;i<=n;i++)
		{
			m1=i;
			for(int j=i+1;j<=n;j++)
			{
				m2=(i+j)>>1;
				w[i][j]=w[i][j-1]+(t[m2]-t[m1])*(m2-i)-(t[m2]-t[m1])*(j-1-m1);
				w[i][j]+=t[j]-t[m2];
				m1=m2;
			}
		}
		
		
		for(int i=1;i<=n;i++)
		{
			f[i][1]=w[1][i];
			p[i][1]=(i+1)>>1;
			for(int j=std::min(i,m);j>1;--j)
			{
				f[i][j]=0x3f3f3f3f;
				if(p[i][j+1])m1=p[i][j+1];
				else m1=i-1;
				for(int k=p[i-1][j];k<=m1;k++)
				{
					if(f[k][j-1]+w[k+1][i]<f[i][j])
					{
						f[i][j]=f[k][j-1]+w[k+1][i];
						p[i][j]=k;
					}
				}
				//printf("%d %d %d\n",i,j,f[i][j]);
			}
		}
		printf("%d",f[n][m]);
	}
}

int main()
{
	fdd::solve();
	return 0;
}
```

# [CF868F] Yet Another Minimization Problem 

[题目链接](https://www.luogu.com.cn/problem/CF868F)

其实做完了`邮局`再来做这一题就会比较舒服了，基本都差不多了。但是四边形不等式仍然无法深刻理解...

交一波，就非常难受地得到了...`TLE`

![lwKQyR.png](https://s2.ax1x.com/2020/01/04/lwKQyR.png)

因为是暴力跳边界（类似莫队），得到只取一段的答案，这题必须还得套上一个分治，使得跳的数量尽量少。

这就十分玄学了。

另外各个过程中的`long long`要注意。

```cpp
#include<bits/stdc++.h>
#define ll long long
namespace fdd
{
	int n,m,l,r,p[100100][23];
	ll s,t[100100],f[100100][23],ct[100100];
	
	ll sum(ll tol,ll tor)
	{
		int k;
		while(tol<l)
		{
			k=t[--l];
			s-=(ct[k]*(ct[k]-1))/2;
			++ct[k];
			s+=(ct[k]*(ct[k]-1))/2;
		}
		while(tor>r)
		{
			k=t[++r];
			s-=(ct[k]*(ct[k]-1))/2;
			++ct[k];
			s+=(ct[k]*(ct[k]-1))/2;
		}
		while(tol>l)
		{
			k=t[l++];
			s-=(ct[k]*(ct[k]-1))/2;
			--ct[k];
			s+=(ct[k]*(ct[k]-1))/2;
		}
		while(tor<r)
		{
			k=t[r--];
			s-=(ct[k]*(ct[k]-1))/2;
			--ct[k];
			s+=(ct[k]*(ct[k]-1))/2;
		}
		//printf("#%d %d %d %d %d\n",ct[1],ct[2],tol,tor,s);
		return s;
	}
	
	void solven(int m,int l,int r,int ln,int rn)
	{
		if(l>r)return;
		int mm,mid=(l+r)>>1;
		for(int k=ln;k<=rn;++k)
		{
			//printf("%d %d %d %d\n",i,j,k,f[k][j-1]+sum(k+1,i));
			if(f[k][m-1]+sum(k+1,mid)<f[mid][m])
			{
				f[mid][m]=f[k][m-1]+sum(k+1,mid);
				mm=k;
			}
		}
		solven(m,l,mid-1,ln,mm);
		solven(m,mid+1,r,mm,rn);
		//printf("#%d %d %d\n",i,j,f[i][j]);
	}
	
	void solve()
	{
		scanf("%d%d",&n,&m);
		for(int i=1;i<=n;i++)scanf("%lld",&t[i]);
		l=r=1;ct[t[1]]=1;
		memset(f,0x3f,sizeof(f));
		
		for(int i=1;i<=n;i++)f[i][1]=sum(1,i);
		
		for(int i=2;i<=m;i++)solven(i,1,n,1,n); 
		
		printf("%lld",f[n][m]);
	}
}

int main()
{
	fdd::solve();
	return 0;
}
```
