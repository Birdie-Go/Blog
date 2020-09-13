# 数学建模国赛2020

## 一点感想和分析

我只是个打杂的。

不过打杂的过程中倒是学会了不少东西。

遗传算法和模拟退火在随机化算法中占据的不同的地位，遗传算法可能更适合于有一定的约束条件，而且每一组方案里各个数字是由关联的，而模拟退火可能更适合约束条件少但规模较大的问题。

当然，我只是按需提供了代码，浅略研究了一下上述两个算法。

遗传算法的代码写成了工程文件，就只贴上模拟退火的代码。

## 模拟退火

就单从这一题来介绍一些模拟退火。

给定初始方案并计算答案，每一轮随机新的方案，若方案更优则更新答案并保留方案，若方案不优则在方案附近计算，并由一定概率保留这个方案，保留概率是轮数以及与答案差值的函数，根据题目设定。

```
#include <bits/stdc++.h>
using namespace std;

double ans[10],ok[10];

double f(int kin,double x)
{
    if (kin==1 || kin==4 || kin==7) return -76.41*x*x+21.98*x-0.6971;
    if (kin==2 || kin==5 || kin==8) return -67.93*x*x+20.21*x-0.6504;
    if (kin==3 || kin==6 || kin==9) return -63.94*x*x+19.57*x-0.6393;
}

int main()
{
    freopen("main.in","r",stdin);
    freopen("main.out","w",stdout);

    double m[10]={0,86.21,86.21,86.21,53.57,53.57,53.57,31.25,31.25,31.25};
    double x[10]={0,0.04,0.04,0.04,0.04,0.04,0.04,0.04,0.04,0.04};
    double ok[10]={0,0.04,0.04,0.04,0.04,0.04,0.04,0.04,0.04,0.04};
    double n[10]={0,13.0,8.0,8.0,13.0,7.0,6.0,6.0,17.0,19.0};
    double g[10]={0,0.0,0.0001,0.0532,0.0,0.0001,0.0912,0.0000,0.0001,0.0822};
    
    for (int i=1;i<=9;i++)
        ans[i]=(m[i]*n[i]*x[i]*(1.0-g[i]))*(1.0-f(i,x[i]));

    srand(time(0));
    int cnt=0;
    while (1)
    {
        for (int i=1;i<=9;i++)
        {
            int r=rand()%11001+4000;
            double now=double(r)/100000.0;
            double res=(m[i]*n[i]*now*(1.0-g[i]))*(1.0-f(i,now));
            if (ans[i]<res)
            {
                ans[i]=res;
                ok[i]=now;
                x[i]=now;
            }
            else
            {
                double k=(double)(rand()%10000)/10000.0;
                if (k<exp(-abs(res-ans[i])/double(i))) x[i]=now;
            }
        }
        cnt++;
        if (cnt>10000000) break;
    }

    for (int i=1;i<=9;i++) printf("%0.5lf\n",ok[i]);

    return 0;
}
```