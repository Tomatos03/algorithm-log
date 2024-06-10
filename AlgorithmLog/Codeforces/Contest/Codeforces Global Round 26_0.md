# Codeforces Global Round 26

## A. Strange Splitting

### 思路

如果数组中每个数都相同答案是NO, 否者总是Yes。因为数组递增，且至少存在两个不同数，所以必然有$a[1] != a[n]$(下标从1开始)，又因为数组长$n > 3$所以总能够得到这样一种染色方案，第二个元素染红色其他元素全染上蓝色。

### 代码

```c++
#include <bits/stdc++.h>

using namespace std;

#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, starts, ends) for(int i = starts; i <= ends; ++i)

void sovle() {
    int n;
    cin >> n;
    int a[n + 1] = {0};
    rep(i, 1, n) cin >> a[i];
    if(a[1] == a[n]) {
        cout << "No" << endl;
        return;
    }
    string ans(n, 'R');
    ans[1] = 'B';
    cout << "Yes" << endl;
    cout << ans << '\n';
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



## B. Large Addition

### 思路

设：两个大数的和为$t$

1. **对于$n > 2$的$t$，因为两个大数相同，所以大数的长度总是$n - 1$​**

2. **t的最高位数位上的数字一定是1**

3. **t的最后一位取值范围为[0,8]**

4. **t的其他数位上的数取值范围为[1, 9]**

满足上述条件总存在如下构造方案，从最高位开始，将`t`的第`i`位减一，第`i - 1`位加10，重复上述过程直到`i`等于最低位

### 代码

```c++
#include <bits/stdc++.h>

using namespace std;

#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, starts, ends) for(int i = starts; i <= ends; ++i)


void sovle() {
    // first digit -> [1]
    // other digit -> [1, 9]
    // last digit -> [0, 8]
    string x;
    cin >> x;
    int n = x.size();
    if(x[0] != '1') {
        cout << "No" << endl;
        return;
    }
    rep(i, 1, n - 2) {
        if(x[i] == '0') {
            cout << "No" << endl;
            return;
        }
    }
    cout << (x[n - 1] != '9' ? "Yes" : "No") << endl;
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

## C1. Magnitude (Easy Version)
### 思路

最多应用一次操作2，枚举每一个可能应用操作2的位置并记录其最小值，设前缀和为$sum_i$最终答案为

​					$(sum_n - sum_i) + abs(sum_i) $

记$sum_i < 0$​​时应用操作2为一个有效的操作2。使用一次有效的操作2后，会导致`i`后的所有$sum_i$整体变大，如果对于`j > i`存在`sum_j < 0`，那么在`j`再次使用操作2，不如仅在`j`点应用操作2更优



### 代码

```c++
#include <bits/stdc++.h>

using namespace std;

#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, starts, ends) for(int i = starts; i <= ends; ++i)

void sovle() {
    int n; 
    cin >> n;
    int a[n + 1] = {0};
    rep(i, 1, n) cin >> a[i];

    long long k = 0, sum = 0;
    rep(i, 1, n) {
        sum += a[i];
        k = min(k, sum);
    }
    // ans = sum_n - sum_i + abs(sum_i)
    // sum_i > 0, ans = sum_n
    // sum_i < 0, ans = sum_n - 2 * sum_i
    cout << (sum - 2 * k) << endl;
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



## C2. Magnitude (Hard Version)
### 思路

如果不存在负数，答案为$2^n$​​​，当存在负数时，从左到右遍历数组，记录`sum_i > 0`的数量，遍历到点

$k,k \in set， set为使sum_i最小的点集$时，对于$j < k, sum_j > 0$的点$j$，既能够使用操作2又能够使用操作1。由于$sum_k = min\{sum_i\}$，在k点应用操作2后不存在$t > k, sum_t < 0$，所以点$k$后的所有点既能够使用操作2又能够使用操作1，所以点$k$对总答案的贡献为$2^{one + (n - k)}$​​。



### 代码
```c++
#include <bits/stdc++.h>

using namespace std;

#define MAXN int(2e5 + 10)
#define MOD int(998244353)
    
#define rep(i, starts, ends) for(int i = starts; i <= ends; ++i)

int pow2[MAXN];

void sovle() {
    int n; 
    cin >> n;
    int a[n + 1] = {0};
    rep(i, 1, n) cin >> a[i];
    long long minn = 0, sum = 0;
    rep(i, 1, n) {
        sum += a[i];
        minn = min(sum, minn);
    }
    if(minn == 0) {
        cout << pow2[n] << endl;
        return;
    }

    int one = 0;
    long long sum2 = 0;
    int ans = 0;
    rep(i, 1, n) {
        sum2 += a[i];
        if(sum2 == minn) {
            ans = (ans + pow2[n - i + one]) % MOD;
        }
        one += sum2 >= 0;
    }
    cout << ans << endl;
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    pow2[0] = 1;
    rep(i, 1, int(2e5)) {
        pow2[i] = pow2[i - 1] * 2;
        pow2[i] %= MOD;
    }
    int t;
    cin >> t;
    while(t-- > 0) {
        sovle();
    }
    return 0;
}
```