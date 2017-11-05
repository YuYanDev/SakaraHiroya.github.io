---
title: HBUT 2nd ACM Contest
date: 2017-05-28 09:32:15
---

恩，菜鸡和大佬之间的差距不是一天两天能追上的，当然思路正确写不出来是不可原谅的。。。。。。总之，很丢人orz

虽然都是大部分是水题，但是还是把这部分分享出来吧w

拿自己的和标程对比，人家的就是写的就是清爽漂亮，哎。。。

<!--more-->
### Problem A:

(1s/32768k)

Lucyma(Lucy 的妈妈)最近很着急，因为Lucy不喜欢吃苹果，但俗话说“每天一苹果，医生远离我”，Lucyma很像让Lucy多吃一点苹果，那怎么办呢？于是Lucyma每天都会按照顺序把水果放到果盘里，如果果盘装满，Lucyma就会将最先放的水果拿出来，以不能浪费粮食的名义，让Lucy吃掉。现在Lucyma一共有n个水果，她想知道这种方法能让lucy最多吃掉几个苹果，你能告诉Lucyma么？

#### 输入描述：
第一行是一个整数T，代表数据组数。(0<T<=20)
每组输入有两行，第一行是两个整数n，m分别代表水果的个数和果盘的容量，用一个空格隔开(0<n,m<=100000)
第二行是一串长度为n的字符串，都是小写字母，其中每个数字都代表一种水果，a代表苹果。

#### 输出描述：
对于每组数据，输出一行，一个整数m，代表lucy最多吃掉苹果的个数。

##### Sample Input
```
2
3 5
aab
5 3
abasd
```
##### Sample Output
```
0
2
```

##### AC Code
``` cpp
#include <iostream>
#include <cstring>
#include <cstdio>
#include <algorithm>
#include <cmath>
#include <ctime>
using namespace std;
const int maxn = 100005;
char s[maxn];
int main()
{
    int T;
    scanf("%d",&T);
    while(T--)
    {
        int n,m;
        scanf("%d %d",&n,&m);
        scanf("%s",s);
        int num=0;
        if(m>=n) printf("0\n");
        else{
            for(int i=0;i<n;i++)
            {
                if(s[i]=='a')
                    num++;
            }
            if(num>=(n-m)) printf("%d\n",n-m);
            else printf("%d\n",num);
        }
    }
    return 0;
}

```


### Problem B:

(1s/32768k)

Lucy最近迷上了看奥运会，她对运动员不抛弃不放弃，以及追求更高，更快，更强的奥运会精神深深折服。Jason也是个奥运迷，有一天，lucy和jason讨论奥运会举办的时间，lucy认为奥运会是每四年举办一次，jason认为奥运会都是在闰年举办的。lucy知道jason的想法是错的，你能帮助lucy么？（第一界奥运会举办时间是在1896年）

#### 输入描述：
输入第一行是一个整数T，代表数据组数。(0<T<=20)
每组第一行是一个N，代表年份(0<n<2017)

#### 输出描述：
每组输出两行，第一行输出是否为奥运会举办的年份，如果是，输出"YES",否则输出"NO"。第二行输出是否为闰年，如果是，输出"YES"，否则输出"NO"；

##### Sample Input
```
1
1900
```
##### Sample Output
```
YES
NO
```

##### AC Code
``` cpp
#include <iostream>
#include <cstring>
#include <cstdio>
#include <algorithm>
#include <cmath>
#include <ctime>
using namespace std;

int ay(int n)
{
    if(n%4==0) return 1;
    else return 0;
}
int yn(int n)
{
    if(n%100==0)
    {
        if(n%400==0) return 1;
        else return 0;
    }
    else
    {
        if(n%4==0) return 1;
        else return 0;
    }
}
int main()
{
    int T;
    scanf("%d",&T);
    while(T--)
    {
        int n;
        scanf("%d",&n);
        if(ay(n)&&n>=1896) printf("YES\n");
        else printf("NO\n");
        if(yn(n)) printf("YES\n");
        else printf("NO\n");
    }
    return 0;
}
```


### Problem C:

(1s/32768k)

Lucy和jason最近刚学了三角形的基本性质，lucy觉得自己学的比jason好，jason不服气了，想考一考lucy，看谁学得更好。于是jason捡来了很多小木棍，问其中能不能找到三根木棍可以组成一个三角形。其实偷偷告诉你lucy学得并不好，但是不能被jason发现了，你能编写个程序来帮lucy回答么？

#### 输入描述：
第一行是一个整数T，代表数据数组。(0<T<=20)
每组数据有两行，第一行数据是一个整数n，代表木棍的数量。第二行是n个整数，i代表每个木棍的长度。(3<=n<100,0<i<100000)

#### 输出描述：
对于每组数据，输出一行，如果有三根木棍可以组成一个三角形，输出"YES",否则输出"NO"。

##### Sample Input
```
3
3
1 2 3
4
1 2 3 4
5
1 2 3 4 5
```
##### Sample Output
```
NO
YES
YES
```

##### AC Code
``` cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <cmath>
using namespace std;
const int maxn = 105;
int a[maxn];
int main()
{
    int T;
    scanf("%d",&T);
    while(T--)
    {
        memset(a,0,sizeof(a));
        int n;
        scanf("%d",&n);
        for(int i=0;i<n;i++)
            scanf("%d",&a[i]);
        sort(a,a+n);
        int flag=0;
        for(int i=0;i<n;i++)
        for(int j=i+1;j<n;j++){
            for(int k=j+1;k<n;k++)
            {
                if((a[i]+a[j])<=a[k]) continue;
                if(a[i]<=(a[k]-a[j])) continue;
                flag=1;
            }
        }
        if(flag) printf("YES\n");
        else printf("NO\n");
    }
    return 0;
}
```

### Problem D:

(1s/32768k)

一天，lucy和jason被传送到了一个从未捡到过的地方，她们唯一要做的事情就是努力活下去。她们惊喜的发现附近有许多可加工的食物。一共有n个食物，使用这些食物可以增加她们的饱和度Yi，但是令人心塞的是，每一种食物都有它相应的时间Xi。如果加工时间未达到，增加的饱食度与它可以增加的最大饱食度的比例和它的实际加工时间占需要加工时间的比例一致。例如 胡萝卜本来可以增加饱食度10，需要加工的时间为2，如果加工时间为1，就只能增加饱食度5了。而且她们的时间是宝贵的，只能有k时间用来加工食物，请问她们加工这些食物之后能增加的最大的饱食度是多少呢？

#### 输入描述：
第一个数字为T，表示样例总数。(0<T<=100)
每个样例的第一行输入为两个整数n和k。(0<n<=1000,0<k<=1000)
每个样例第二行为n个整数，第i个数字表示第i种食物的加工时间Xi。(0<Xi<=100)
每个样例的第三行为n个整数，第i个数字表示第i种食物可以增加的最大饱食度Yi(0<Yi<100)

#### 输出描述：
每个样例输出一个数字。每个数字占一行，表示lucy和jason最多能增加的饱食度，保留两位小数。

##### Sample Input
```
1
3 2
1 1 2
10 10 5
```
##### Sample Output
```
20.00
```

##### AC Code
``` cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cstdlib>
#include <ctime>

using namespace std;
const int maxn = 1005;
struct food
{
    int ti,d;
    food(int a,int b)
    {   ti = a; d = b;  }
    food(){}
}data[maxn];
bool cmp(food a, food b)
{
    double x = (double)a.d/a.ti,y = (double)b.d/b.ti;
    return x > y;
}

int kase;
void datamake()
{
    srand(time(0));
    kase = 100;
    printf("%d\n",kase);

    while(kase--)
    {
        int n = rand()%1000 + 1;
        int k = rand()%1000 + 1;
        printf("%d %d\n",n,k);
        for(int i = 0; i < n ; i++)
        {
            int casei = rand()%100+1;
            i == 0 ? printf("%d",casei) : printf(" %d",casei);
        }
        printf("\n");
        for(int i = 0; i < n ; i++)
        {
            int casei = rand()%100+1;
            i == 0 ? printf("%d",casei) : printf(" %d",casei);
        }
        printf("\n");
    }
}


int main()
{
    int n,k;
    scanf("%d",&kase);
    while(kase--)
    {
        scanf("%d %d",&n,&k);
        for(int i = 0; i < n; i++)
            scanf("%d",&data[i].ti);
        for(int i = 0; i < n ; i++)
            scanf("%d",&data[i].d);
        sort(data,data+n,cmp);
        double ans = 0;
        int use = 0;
        for(int i = 0; i < n ; i++)
        {
            if(use + data[i].ti <= k)
            {
                ans += data[i].d;
                use += data[i].ti;
            }
            else
            {
                if(use < k)
                {
                    double x = (double)(k-use)/data[i].ti;
                    ans += (double)x*data[i].d;
                    use = k;
                }
            }
        }
        printf("%.2f\n",ans);
    }
    return 0;
}

```


### Problem E: (<del>简直欺负我不玩阴阳师</del>)

(1s/32768k)

到五一假期的时候阴阳师又推出了这一活动，一张蓝色卡可以抽一次奖（每次用蓝色卡就会得到一点积分，满足10点积分额外送一张蓝色卡）抽奖是一个吸欧气的过程，每抽520次就能抽到一个ssr式神。ssr式神一共有10种，请问抽到相同ssr式神的概率有多少。

#### 输入描述
第一行是一个整数T，表示数据组数，(1<=T<=10000)
每组输入一个整数n，表示初始蓝色卡个数。(0<n<=1000000000)

#### 输出描述
输出抽到相同式神的概率。（算得的结果小数点保留6位，概率的最大值为1）

##### Sample Input
```
2
123
2560
```
##### Sample Output
```
0.000000
0.697600
```

##### AC Code
``` cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
using namespace std;

double fact[13], cnt[13];

void init(){
    fact[0] = cnt[0] = 1.0;
    for(int i = 1; i <= 10; i++){
        fact[i] = fact[i-1] * i;
        cnt[i] = cnt[i-1] * 10;
    }
}

int main(){
    init();
    int T;
    scanf("%d", &T);
    while(T--){
        int n;
        scanf("%d", &n);
        int left = 0, ans = 0;
        while(n){
            ans += n;
            left += n;
            n = left/10;
            left %= 10;
        }
        ans /= 520;
     	if(ans > 10) printf("1.000000\n");
        else if(ans == 10) printf("%.6lf\n", 1.0-fact[9]/cnt[9]);
        else printf("%.6lf\n", 1.0-fact[10]/fact[10-ans]/cnt[ans]);
    }
    return 0;
}
```

#### Problem F && Problem G && Problem H:

略（看不懂）
