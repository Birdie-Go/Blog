# ICPC Asia Nanning 2017

A
```
#include <bits/stdc++.h>
using namespace std;

int main() {
    int T;scanf("%d",&T);
    while (T--) {
        int x;scanf("%d",&x);
        for (int i=1;i<=x;i++)
            puts("Abiyoyo, Abiyoyo.");
        puts("Abiyoyo, yo yoyo yo yoyo.");
        puts("Abiyoyo, yo yoyo yo yoyo.");
    }
}
```

J
```
include <bits/stdc++.h>
using namespace std;

int cnt[5];

int main() {
    int T;scanf("%d",&T);
    while (T--) {
        int n;scanf("%d",&n);
        memset(cnt,0,sizeof(cnt));
        for (int i=1;i<=2*n;i++) {
            int x;scanf("%d",&x);
            x%=3;
            cnt[x]++;
        }
        int flag=0;
        if (cnt[0]>n) flag=1;
        else if (cnt[0]==2) {
            if (cnt[1]%2==0) flag=1;
        }
        else if (cnt[0]<2) {
            if (cnt[1]>0 && cnt[2]>0) flag=1;
        }
        if (flag) puts("NO");else puts("YES");
    }
}
```

F
```
#include <bits/stdc++.h>
using namespace std;

const int N = 55, M = 170;

int T, a[55], n, tmp[N], pre[N];
int pow2[M][N];
char ch[55];

int cmp(int b[]) {
    for (int i = max(b[0], a[0]); i; --i) {
        if (a[i] > b[i])
            return 1;
        if (a[i] < b[i])
            return 0;
    }
    return 1;
}

void muti(int s[]) {
    memset(tmp, 0, sizeof(tmp));
    tmp[0] = s[0];
    for (int i = 1; i <= s[0]; ++i) {
        tmp[i] += s[i] * 2;
        tmp[i + 1] += tmp[i] / 10;
        tmp[i] %= 10;
    }
    if (tmp[tmp[0] + 1]) tmp[0]++;
    for (int i = 0; i <= tmp[0]; ++i)
        s[i] = tmp[i];
}

void init() {
    pow2[0][0] = 1; pow2[0][1] = 1;
    for (int i = 1; i < M; ++i) {
        for (int j = 0; j <= pow2[i - 1][0]; ++j)
            pow2[i][j] = pow2[i - 1][j];
        muti(pow2[i]);
    }
}

int solve() {
    int l = 0, r = M - 1, mid, ret;
    for (; l <= r;) {
        mid = (l + r) >> 1;
        if (cmp(pow2[mid])) {
            ret = mid; l = mid + 1;
        }
        else r = mid - 1;
    }
    return ret;
}

void print(int b[]) {
    for (int i = b[0]; i; --i)
        printf("%d", b[i]);
    puts("");
}

int main() {
    scanf("%d" ,&T);
    init();
    for (; T; T--) {
        scanf("%s", ch + 1);
        n = strlen(ch + 1);
        memset(a, 0, sizeof(a));
        for (int i = 1; i <= n; ++i)
            a[i] = ch[n - i + 1] - '0';
        a[0] = n;
        int ans = solve();
        // printf("%d\n", ans);
        for (int i = pow2[ans][0]; i; --i)
            printf("%d", pow2[ans][i]);
        puts("");
    }
}
```

L
```
#include<iostream>
#include<vector>
#include<cstdio>
#include<set>
#include<cstring>
using namespace std;
typedef long long ll;
class Bignum{ // keep number>0 while the whole calculation
private:
	vector<int>c;
	void assign(long long x){
		c.clear();
		while(x){
			c.push_back(x%10);
			x/=10;
		}
	}
	void assign(const string &str){
		c.clear();
		int s=str.size()-1;
		for(;s>=0;--s){
			c.push_back(str[s]-'0');
		}
	}
	void erase_front_zero(){
		int s=c.size()-1;
		while(s>0&&c[s]==0) --s;
		c.resize(s+1);
	}
public:
	Bignum(){
		assign(0);
	}
	Bignum(long long x){
		assign(x);
	}
	Bignum(const string &str){
		assign(str);
	}
	Bignum operator + (const Bignum &rhs) const{
		Bignum tmp;
		vector<int> &vec=tmp.c;
		for(int i=0,d=0;;++i){
			if(i>=rhs.c.size()&&i>=c.size()&&!d) break;
			int add=0;
			if(i<rhs.c.size()) add+=rhs.c[i];
			if(i<c.size()) add+=c[i];
			vec.push_back((d+add)%10);
			d=(d+add)/10;
		}
		return tmp;
	}
	Bignum operator + (long long rhs) const{
		Bignum tmp(*this);
		int add=0;
		for(int i=0;rhs||add;++i,rhs/=10){
			if(i<tmp.c.size()){
				tmp.c[i]+=rhs%10+add;
			} else {
				tmp.c.push_back(rhs%10+add);
			}
			add=tmp.c[i]/10;
			tmp.c[i]%=10;
	}
		return tmp;
	}
	Bignum operator - (const Bignum &rhs) const{
		Bignum tmp;
		vector<int> &vec=tmp.c;
		vec.assign(c.begin(),c.end());
		for(int i=0,d=0;;++i){
			if(i>=rhs.c.size()&&!d) break;
			if(i<rhs.c.size()) vec[i]-=rhs.c[i];
			vec[i]-=d;d=0;
			if(vec[i]<0){
				vec[i]+=10;
				d=1;
			}
		}
		tmp.erase_front_zero();
		return tmp;
	}
	Bignum operator * (const Bignum &rhs) const{
		Bignum tmp;
		vector<int> &vec=tmp.c;
		vec.resize(c.size()+rhs.c.size());
		for(int i=0;i<c.size();++i)
			for(int j=0;j<rhs.c.size();++j){
				vec[i+j]+=c[i]*rhs.c[j];
			}
		for(int i=0,d=0;i<vec.size();++i){
			vec[i]+=d;d=0;
			if(vec[i]>=10){
				d=vec[i]/10;
				vec[i]%=10;
			}
		}
		tmp.erase_front_zero();
		return tmp;
	}
	Bignum operator / (const long long &rhs) const{
		string ans("0");
		long long now=0;
		int n=c.size()-1;
		for(int i=n;i>=0;--i){
			now=now*10+c[i];
			if(now>=rhs){
				ans+=now/rhs+'0';
				now%=rhs;
				n=i-1;break;
			}
		}
		for(int i=n;i>=0;--i){
			now=now*10+c[i];
			ans+=now/rhs+'0';
			now%=rhs;
		}
		Bignum tmp(ans);
		tmp.erase_front_zero();
		return tmp;
	}
	Bignum operator = (const Bignum &rhs){
		if(&rhs==this) return *this;
		c.assign(rhs.c.begin(),rhs.c.end());
		return *this;
	}
    bool operator < (const Bignum &rhs) {
        if (rhs.c.size()!=c.size()) return c.size()<rhs.c.size();
        for (int i=c.size()-1;i>=0;i--) {
            if (c[i]!=rhs.c[i]) return c[i]<rhs.c[i];
        }
        return false;
    }
	friend istream &operator >> (istream &in,Bignum &rhs){
		string tmp;
		in>>tmp;
		rhs.assign(tmp);
		return in;
	}
	friend ostream &operator << (ostream &out,const Bignum &rhs){
		int s=rhs.c.size()-1;
		for(;s>=0;--s){
			out<<rhs.c[s];
		}
		return out;
	}
};

const int maxn=1000+10;
Bignum f[maxn];

void ready() {
    f[0]=Bignum(0);
    f[1]=Bignum(3);
    for (int i=2;i<=1000;i++) {
        f[i]=f[i-1]*6-f[i-2]+2;
    }
}

char ch[maxn];

int main(){
    ready();

	int T;cin>>T;
    while (T--) {
        cin>>ch;
        Bignum now(ch);
        for (int i=0;i<=1000;i++) {
            if (f[i]<now) continue;
            cout<<f[i]<<endl;
            break;
        }
    }
}
```

E
```
#include <bits/stdc++.h>
using namespace std;
typedef unsigned long long LL;

const double eps=1e-8;
map <pair<LL,LL>,double> f;
map <pair<LL,LL>,bool> vis;

int cmp(double x) {
    if (fabs(x)<eps) return 0;
    if (x>0) return 1;
    return -1;
}

LL fas(LL a,LL k) {
    if (k==0) return 1ll;
    LL now=fas(a,k/2);
    now=now*now;
    if (k%2==1) now=now*a;
    return now;
}

void dfs(LL r,LL k,double p) {
    if (k==fas(2ll,r)) f[{r-1,k/2}]=f[{r,k}]*(1.0-p);
}

int main() {
    int T;scanf("%d",&T);
    while (T--) {
        LL r,k;double p;
        scanf("%llu%llu%lf",&r,&k,&p);
        if (cmp(p-0.5)==-1) k=fas(2ll,r)-k+1ll;
        f.clear();vis.clear();

        f[{r,k}]=1.0;
        vis[{r,k}]=1;
        queue <pair<LL,LL>> que;
        que.push({r,k});
        while (!que.empty()) {
            pair <LL,LL> now=que.front();
            LL r=now.first,k=now.second;
            que.pop();
            if (r==0) break;
            if (k==fas(2ll,r)) {
                pair <LL,LL> nex={r-1,k/2ll};
                f[nex]+=f[{r,k}]*(1.0-p);
                if (vis[nex]!=1) {
                    vis[nex]=1;
                    que.push(nex);
                }
            }
            else {
                if ((k-1)%2==0) {
                    pair <LL,LL> nex={r-1ll,(k-1ll)/2ll+1ll};
                    f[nex]+=f[{r,k}]*p;
                    if (vis[nex]!=1) {
                        vis[nex]=1;
                        que.push(nex);
                    }
                }
                else {
                    pair <LL,LL> nex={r-1ll,(k-1ll)/2ll+2ll};
                    f[nex]+=f[{r,k}]*p*p;
                    if (vis[nex]!=1) {
                        vis[nex]=1;
                        que.push(nex);
                    }

                    nex={r-1ll,(k-1ll)/2ll+1ll};
                    f[nex]+=f[{r,k}]*p*(1-p);
                    if (vis[nex]!=1) {
                        vis[nex]=1;
                        que.push(nex);
                    }
                }
            }
        }
        printf("%.6lf\n",f[{0,1}]);
    }
}
```

L
```
#include<iostream>
#include<vector>
#include<cstdio>
#include<set>
#include<cstring>
using namespace std;
typedef long long ll;
class Bignum{ // keep number>0 while the whole calculation
private:
	vector<int>c;
	void assign(long long x){
		c.clear();
		while(x){
			c.push_back(x%10);
			x/=10;
		}
	}
	void assign(const string &str){
		c.clear();
		int s=str.size()-1;
		for(;s>=0;--s){
			c.push_back(str[s]-'0');
		}
	}
	void erase_front_zero(){
		int s=c.size()-1;
		while(s>0&&c[s]==0) --s;
		c.resize(s+1);
	}
public:
	Bignum(){
		assign(0);
	}
	Bignum(long long x){
		assign(x);
	}
	Bignum(const string &str){
		assign(str);
	}
	Bignum operator + (const Bignum &rhs) const{
		Bignum tmp;
		vector<int> &vec=tmp.c;
		for(int i=0,d=0;;++i){
			if(i>=rhs.c.size()&&i>=c.size()&&!d) break;
			int add=0;
			if(i<rhs.c.size()) add+=rhs.c[i];
			if(i<c.size()) add+=c[i];
			vec.push_back((d+add)%10);
			d=(d+add)/10;
		}
		return tmp;
	}
	Bignum operator + (long long rhs) const{
		Bignum tmp(*this);
		int add=0;
		for(int i=0;rhs||add;++i,rhs/=10){
			if(i<tmp.c.size()){
				tmp.c[i]+=rhs%10+add;
			} else {
				tmp.c.push_back(rhs%10+add);
			}
			add=tmp.c[i]/10;
			tmp.c[i]%=10;
	}
		return tmp;
	}
	Bignum operator - (const Bignum &rhs) const{
		Bignum tmp;
		vector<int> &vec=tmp.c;
		vec.assign(c.begin(),c.end());
		for(int i=0,d=0;;++i){
			if(i>=rhs.c.size()&&!d) break;
			if(i<rhs.c.size()) vec[i]-=rhs.c[i];
			vec[i]-=d;d=0;
			if(vec[i]<0){
				vec[i]+=10;
				d=1;
			}
		}
		tmp.erase_front_zero();
		return tmp;
	}
	Bignum operator * (const Bignum &rhs) const{
		Bignum tmp;
		vector<int> &vec=tmp.c;
		vec.resize(c.size()+rhs.c.size());
		for(int i=0;i<c.size();++i)
			for(int j=0;j<rhs.c.size();++j){
				vec[i+j]+=c[i]*rhs.c[j];
			}
		for(int i=0,d=0;i<vec.size();++i){
			vec[i]+=d;d=0;
			if(vec[i]>=10){
				d=vec[i]/10;
				vec[i]%=10;
			}
		}
		tmp.erase_front_zero();
		return tmp;
	}
	Bignum operator / (const long long &rhs) const{
		string ans("0");
		long long now=0;
		int n=c.size()-1;
		for(int i=n;i>=0;--i){
			now=now*10+c[i];
			if(now>=rhs){
				ans+=now/rhs+'0';
				now%=rhs;
				n=i-1;break;
			}
		}
		for(int i=n;i>=0;--i){
			now=now*10+c[i];
			ans+=now/rhs+'0';
			now%=rhs;
		}
		Bignum tmp(ans);
		tmp.erase_front_zero();
		return tmp;
	}
	Bignum operator = (const Bignum &rhs){
		if(&rhs==this) return *this;
		c.assign(rhs.c.begin(),rhs.c.end());
		return *this;
	}
    bool operator < (const Bignum &rhs) {
        if (rhs.c.size()!=c.size()) return c.size()<rhs.c.size();
        for (int i=c.size()-1;i>=0;i--) {
            if (c[i]!=rhs.c[i]) return c[i]<rhs.c[i];
        }
        return false;
    }
	friend istream &operator >> (istream &in,Bignum &rhs){
		string tmp;
		in>>tmp;
		rhs.assign(tmp);
		return in;
	}
	friend ostream &operator << (ostream &out,const Bignum &rhs){
		int s=rhs.c.size()-1;
		for(;s>=0;--s){
			out<<rhs.c[s];
		}
		return out;
	}
};

const int maxn=1000+10;
Bignum f[maxn];

void ready() {
    f[0]=Bignum(0);
    f[1]=Bignum(3);
    for (int i=2;i<=1000;i++) {
        f[i]=f[i-1]*6-f[i-2]+2;
    }
}

char ch[maxn];

int main(){
    ready();

	int T;cin>>T;
    while (T--) {
        cin>>ch;
        Bignum now(ch);
        for (int i=0;i<=1000;i++) {
            if (f[i]<now) continue;
            cout<<f[i]<<endl;
            break;
        }
    }
}
```