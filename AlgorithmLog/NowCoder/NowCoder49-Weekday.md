# NowCoder-49-Weekday

## **A -嘤嘤不想做计几喵**

## 思路

按照题目给出的公式直接计算，注意使用64位整数，乘法会造成精度溢出

### 代码

```c++
#include <bits/stdc++.h>

using namespace std;

#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, starts, ends) for(int i = starts; i <= ends; ++i)

typedef long long int64;

void sovle() {
    int64 a, b;
    cin >> a >> b;
    cout << ((a - b) - 10 * b) << '\n';
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    sovle();
    return 0;
}
```



## **B - 嘤嘤不想打怪兽喵**

### 思路

按照题意模拟。使用$pre$记录分裂直接的数量，$num$表示当前数量，$cnt$使用魔法的次数

### 代码

```c++
#include <bits/stdc++.h>

using namespace std;

#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, starts, ends) for(int i = starts; i <= ends; ++i)

typedef long long int64;

void sovle() {
    int h;
    cin >> h;
    int num = 1;
    int64 cnt = 0;
    while(h > 0) {
        int64 pre = num;
        cnt += num;
        h >>= 1;
        num = pre * 2;
    }
    cout << cnt << endl;
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    sovle();
    return 0;
}
```


## **C - 嘤嘤不想买东西喵**

### 思路

让$dp[i]$表示以$nums[i]$结尾的最大省钱数，有如下转移方程：

$dp[i] = max(dp[i], 0) + a[i]$


### 代码

```c++
#include <bits/stdc++.h>

using namespace std;

#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, starts, ends) for(int i = starts; i <= ends; ++i)

typedef long long int64;

void sovle() {
    int n, x;
    cin >> n >> x;
    int a[n + 1] = {0};
    rep(i, 1, n) cin >> a[i];
    int64 cnt = 0, sum = 0;
    rep(i, 1, n) {
        int v = a[i] - x;
        sum = max(sum, 0LL) + v;
        cnt = max(sum, cnt);
    }
    cout << cnt << endl;
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    sovle();
    return 0;
}
```


## **D - 嘤嘤不想求异或喵**

### 思路

$f(l, r)$的异或和可以转化为$f(1, l - 1) \oplus f(1, r)$，$f(i, j)$表示$[i, j]$的异或和

#### 思路1：

考虑每一位的异或和，假设当前二进制位为第$i$位(从右往左并且从1开始数)，二进制中1的个数为

$cnt = \lfloor \frac{x}{2^i} \rfloor \times 2^{i - 1} + max(0, x \% 2^i - 2^{i - 1} + 1)$

#### 思路2

考虑一个数二进制位的第一位和第二位，$f(1, x)$的异或和有如下规律
$$
x \% 4 = 0, f(1,x) = x\\
x \% 4 = 1, f(1,x) = 1\\
x \% 4 = 2, f(1,x) = x + 1\\
x \% 4 = 3, f(1,x) = 0
$$


### 代码

#### 方法一

```c++
#include <bits/stdc++.h>

using namespace std;

#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, starts, ends) for(int i = starts; i <= ends; ++i)

typedef long long int64;

int64 f(int64 x) {
    int64 res = 0;
    for(int i = 1; i <= 62; ++i) {
        int64 t = (1LL << i), k = t / 2;
        int64 v = x / t, u = max((x % t) - k + 1, 0LL);
        int64 cnt = v * k + u;
        res += (cnt & 1) ? k : 0;
    }
    return res;
}

void sovle() {
    int64 l, r;
    cin >> l >> r;
    int64 ans = f(r) ^ f(l - 1);
    cout << ans << endl;
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    int t;
    cin >> t;
    while(t-- > 0) {
        sovle();
    }
    return 0;
}
```

#### 方法2

```c++
#include <bits/stdc++.h>

using namespace std;

#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, starts, ends) for(int i = starts; i <= ends; ++i)

typedef long long int64;

int64 f(int64 x) {
    int64 v = x % 4;
    if(v == 0) return x;
    if(v == 1) return 1;
    if(v == 2) return x + 1;
    return 0;
}

void sovle() {
    int64 l, r;
    cin >> l >> r;
    int64 ans = f(r) ^ f(l - 1);
    cout << ans << endl;
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    int t;
    cin >> t;
    while(t-- > 0) {
        sovle();
    }
    return 0;
}
```

