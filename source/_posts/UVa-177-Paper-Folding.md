---
title: '[UVa 177]Paper Folding'
date: 2019-12-11 01:01:06
categories: OI
tags:
- OI
- UVa
- 码农
- 模拟
- 趣题
---

[题目链接（洛谷）](https://www.luogu.com.cn/problem/UVA177)

（UVa还得科学上网，但debug挺好用可以面向数据编程）

超级好玩的一道题！大概意思就是把纸不停对折然后展开，每次展开夹角为90度。题目本身不难，拿一张纸摆弄一下就出来了。

考虑如何用科学的方式来描述形态，想了想，就用左转和右转来表示，就可以转换为对链操作了。

大概规律是这样的，例如有：

> _|

想象它是两层的，折叠转角在右上角，那么现在将它展开：

> |_
> _|

啊太丑了但是大概就这意思。

第一个序列就是`L`，第二个的序列就是`LLR`。

再多试一下可以发现规律，展开一次就是把原来的序列翻转再反转（顺序翻转，LR互换），然后中间加一个`L`，接在原序列后面。

这没问题，我用了`String`来模拟，也很好写。

输出就比较繁琐了，完全就是码农行为，我没想到什么优美的方法，就暴力模拟了。注意题目要求的起点。就从定义的起点开始顺着排就行。

其实到这时候也没花费多长时间，剩下的时间，大概一个小时，我就在和输出&OJ斗智斗勇。首先要注意输出，什么时候折了换行什么时候不换要搞清楚。如果输出正确，打印出来是非常漂亮的分形图像。很像`bilibili`的up主`Solara570`的头像。

![](https://pic3.superbed.cn/item/5defd3cd1f8f59f4d6075721.png)

因为这神仙UVa竟然对回车空格都敏感，这题每一行就算后面没有内容我都补齐了空格，所以不对，显示`Presentation error`，我把答案下载下来都看了好久好久...

反正就这样改了好久好久...

[Vjudge提交记录](https://vjudge.net/solution/23285320)

```cpp
#include<bits/stdc++.h>

namespace fdd
{

	int n,maxlen[2000];
	char t[2000][2000];
	std::string s1,s2;

	int solve()
	{
		int l;
		s1=s2="";
		memset(t,0,sizeof(t));
		memset(maxlen,0,sizeof(maxlen));
		std::cin>>n;
		if(!n)return 0;
		for(int i=0;i<n;i++)
		{
			s2=s1;
			l=s2.length();
			std::reverse(s2.begin(),s2.end());
			for(int j=0;j<l;j++)
			{
				if(s2[j]=='l')s2[j]='r';
				else s2[j]='l';
			}
			s1+='l'+s2;
		}
		int x=1000,y=1000,maxx=1000,maxy=1000,minx=1000,miny=1000,fa=0;//l 0 r 1 up 2 down 3
		l=s1.length();
		t[x][y]='_';
		for(int i=0;i<l;i++)
		{
			//printf("#%d %d %d\n",i,x,y);
			if(s1[i]=='l')
			{
				if(fa==0)
				{
					fa=3;
					x=x+1;
					//y=y+1;
					t[x][y]='|';
				}
				else if(fa==1)
				{
					fa=2;
					x-=1;
					y-=1;
					t[x][y]='|';
				}
				else if(fa==2)
				{
					fa=0;
					x+=1;
					//y-=1;
					t[x][y]='_';
				}
				else if(fa==3)
				{
					fa=1;
					x-=1;
					y+=1;
					t[x][y]='_';
				}
			}
			else
			{
				if(fa==0)
				{
					fa=2;
					x=x+1;
					y=y-1;
					t[x][y]='|';
				}
				else if(fa==1)
				{
					fa=3;
					x-=1;
					//y+=1;
					t[x][y]='|';
				}
				else if(fa==2)
				{
					fa=1;
					x-=1;
					//y-=1;
					t[x][y]='_';
				}
				else if(fa==3)
				{
					fa=0;
					x+=1;
					y+=1;
					t[x][y]='_';
				}
			}
			maxx=std::max(maxx,x);
			minx=std::min(minx,x);
			maxy=std::max(maxy,y);
			miny=std::min(miny,y);
			maxlen[y]=std::max(maxlen[y],x);
		}
		//std::cout<<s1<<std::endl;
		for(int i=maxy;i>=miny;i--)
		{
			for(int j=minx;j<=maxlen[i];j++)printf("%c",t[j][i]==0?' ':t[j][i]);
			printf("\n");
		}
		printf("^\n");
		return 1;
	}
}

int main()
{
	while(fdd::solve());
	return 0;
}

```
