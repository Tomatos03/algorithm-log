# NowCoder-48-Weekday

## **A -小红的整数自增**

## 思路

只能进行加1操作，最终要使得三个数相等的最小操作数为将，所有的数变为3个数中最大的那个数

### 代码

```c++
#include <bits/stdc++.h>

using namespace std;

#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, starts, ends) for(int i = starts; i <= ends; ++i)

typedef long long int64;

void sovle() {
    int a[3];
    cin >> a[0] >> a[1] >> a[2];
    int maxn = max(max(a[0], a[1]), a[2]);
    int ans = 0;
    rep(i, 0, 2) {
        ans += maxn - a[i];
    }
    cout << ans << endl;
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    sovle();
    return 0;
}
```



## **B - 小红的伪回文子串（easy）**

### 思路

给定数据范围较小，考虑直接暴力枚举

### 代码

```c++
#include <bits/stdc++.h>

using namespace std;

#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, starts, ends) for(int i = starts; i <= ends; ++i)

typedef long long int64;

void sovle() {
    string s;
    cin >> s;
    int n = s.size();
    s = '@' + s;
    int64 ans = 0;
    rep(i, 2, n) {
        for(int j = 1; j + i - 1 <= n; ++j) {
            int l = j, r = j + i - 1;
            while(l < r) {
                ans += s[l] != s[r];
                ++l;
                --r;
            }
        }
    }
    cout << ans << '\n';
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    sovle();
    return 0;
}
```


## **C - 小红的01串取反**

### 思路

一种操作数最小的可行方案为：从$1$枚举到$n-1$，如果$a[i] == b[i]$不处理，如果$a[i] \neq b[i]$将$a[i]$和$a[i + 1]$取反。枚举完后检查字符串a是否等于字符串b，如果不相等则不存在任何操作方案使a等于b


### 代码

```c++
#include <bits/stdc++.h>

using namespace std;

#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, starts, ends) for(int i = starts; i <= ends; ++i)

typedef long long int64;

struct node {
    int l, r;
};

void sovle() {
    int n;
    cin >> n;
    string a, b;
    cin >> a >> b;
    vector<node> ans;
    for(int i = 0; i + 1 < n; ++i) {
        if(a[i] == b[i]) continue;

        a[i] = a[i] == '0' ? '1' : '0';
        a[i + 1] = a[i + 1] == '0' ? '1' : '0';
        ans.push_back({i + 1, i + 2});
    }
    bool ok = true;
    for(int i = 0; i < n; ++i) {
        ok &= a[i] == b[i];
    }
    if(!ok) {
        cout << -1 << '\n';
        return;
    }
    cout << int(ans.size()) << '\n';
    for(node e : ans) {
        cout << e.l << " " << e.r << '\n';
    }
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    sovle();
    return 0;
}
```


## **D - 小红的乘2除2**

### 思路

暴力枚举乘2除2的位置, 有如下几种情况,

1. 乘2除2在同一个位置上
2. 乘2除2的位置距离仅相差1
3. 乘2除2的位置距离相差1以上

计算原数组的陡峭值$cnt$，找到上述每一种情况最小的陡峭值变化量$\Delta x$，最终答案为$\Delta x +cnt$,

设某个位置变化前的数为$x$，变化后的数为$x^{‘}$，其相邻位置两数为$y_1, y_2$，则有：

$\Delta x = |x^{’} - y_1| + |x^{’} - y_2| - |x - y_1| - |x - y_2|$

对于上述第3种情况还需要维护，前缀中除2变化的最小值和后缀中除2变化的最小值


### 代码

```c++
#include <bits/stdc++.h>

using namespace std;

#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, starts, ends) for(int i = starts; i <= ends; ++i)

typedef long long int64;

int n;
int a[MAXN] = {0};

int64 f(int pos, int val) {
    int64 r = 0;
    if(pos + 1 <= n){ 
        r += abs(val - a[pos + 1]);
    }
    if(pos - 1 >= 1) {
        r += abs(val - a[pos - 1]);
    }
    return r;
}

void sovle() {
    cin >> n;
    rep(i, 1, n) cin >> a[i];
    int64 ans = 0;
    rep(i, 1, n - 1) {
        ans += abs(a[i] - a[i + 1]);
    }
    int64 c = INT_MAX;
    int64 pre[n + 2] = {0};
    int64 suf[n + 2] = {0};
    suf[n + 1] = pre[0] = INT_MAX;

    rep(i, 1, n - 2) {
        pre[i] = min(pre[i - 1], f(i, a[i] * 2) - f(i, a[i]));
    }
    for(int i = n; i >= 2; --i) {
        suf[i] = min(suf[i + 1], f(i, a[i] * 2) - f(i, a[i]));
    }

    rep(i, 1, n) {
        int64 v = f(i, a[i] / 2 * 2) - f(i, a[i]);
        c = min(c, v);
    }
    rep(i, 1, n) {
        int b[] = {i - 1, i + 1};
        rep(j, 0, 1) {
            int k = b[j];
            if(k < 1 || k > n) continue;

            int64 r3 = f(i, a[i]);
            int64 r4 = f(k, a[k]);
            int64 d1 = r3 + r4 - abs(a[i] - a[k]);

            int pre_v1 = a[i], pre_v2 = a[k];
            a[i] /= 2, a[k] *= 2;

            int64 r1 = f(i, a[i]);
            int64 r2 = f(k, a[k]);
            int64 d2 = r1 + r2 - abs(a[i] - a[k]);

            c = min(c, d2 - d1);

            a[i] = pre_v1, a[k] = pre_v2;
        }

        int64 r1 = f(i, a[i]);
        int64 r2 = f(i, a[i] / 2);
        int64 d1 = r2 - r1;

        int64 p1 = i - 2 >= 1 ? pre[i - 2] : INT_MAX;
        int64 p2 = i + 2 <= n ? suf[i + 2] : INT_MAX;
        c = min(c, d1 + min(p1, p2));
    }
    cout << (ans + c) << '\n';
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    sovle();
    return 0;
}
```

