# 阿里云超级码力2020A

## 题目大意
给定一个长度为$n(n \leq 10^6)$且由数字$0-9$构成的字符串，取一个子串，令出现次数最多的数字为$a$，最少的为$b$，求$a$的次数减去$b$的次数最大。

## 解题思路

最开始的思路是，枚举$a$和$b$，通过$DP$计算答案，结果$TLE$，可能是因为常数过大吧。

后来换一个思路，只枚举最小值。我们先观测一下答案是什么：
$$cnt_a[i]-cnt_b[i]-(cnt_a[j]-cnt_b[j])$$

对于$cnt_a[i]-cnt_b[i]$，我们是已知的，因此我们只需要最小化$(cnt_a[j]-cnt_b[j])$。当然细节有点多，就不赘述了。

```
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    /**
     * @param s: number string
     * @return: Find the key
     */
    int key(string &s) {
        // write your code here
        int L=s.size(),ans=0;
        int Min[10]={0},cnt[10]={0},can[10]={0};
        for (int a=0;a<10;a++)
        {
            int tot=0;
            for (int j=0;j<10;j++) Min[j]=0,cnt[j]=0,can[j]=0;
            for (int i=0;i<L;i++)
            {
                if (s[i]-48==a)
                {
                    tot++;
                    for (int j=0;j<10;j++)
                        if (tot>0 && j!=a)
                            ans=max(ans,cnt[j]-tot-can[j]);
                    for (int j=0;j<10;j++) can[j]=Min[j];
                    for (int j=0;j<10;j++) Min[j]=min(Min[j],cnt[j]-tot);
                }
                else
                {
                    cnt[s[i]-48]++;
                    if (tot>0) ans=max(ans,cnt[s[i]-48]-tot-can[s[i]-48]);
                }
                
            }
        }
        return ans;
    }
};

int main() {
    freopen("main.in","r",stdin);
    freopen("main.out","w",stdout);

    string s="11100";
    Solution a;
    printf("%d\n",a.key(s));

    return 0;
}
```