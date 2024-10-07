# NowCoder-97-Beginer

## **A - tb的区间问题**

## 思路

由于每一次操作不是删除开头，就是删除末尾，所以我们可以枚举开头或结尾删除的元素的个数。为了快速求解最后剩下的元素的和，可以预处理出数组的前缀和数组。

### 代码

```c++
#include <bits/stdc++.h>
 
using namespace std;
 
#define endl '\n'
#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, start, end) for(int i = start; i <= end; ++i)
#define erp(i, end, start) for(int i = end; i >= start; --i)
 
using i64 = long long;
using pii = pair<int, int>;
using pll = pair<i64, i64>;
 
void solve() {
    int n, k;
    cin >> n >> k;
    int a[n + 1] = {0};
    rep(i, 1, n) cin >> a[i];
    i64 res = 0;
    vector<i64> pre(n + 1, 0);
    rep(i, 1, n) pre[i] = pre[i - 1] + a[i];
    rep(i, 1, k) {
        int j = n - (k - i);
        res = max(res, pre[j] - pre[i]);
    }
    cout << res << endl;
}
 
int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    solve();
    return 0;
}
```



## **B - tb的字符串问题**

### 思路

需要删除的子串没有公共字符，使用栈模拟正序生成字符串过程，当存在栈顶字符与即将要添加的字符组合成的字符串是需要删除的子串，就将栈顶弹出并放弃本次字符的压入。

### 代码

```c++
#include <bits/stdc++.h>

using namespace std;

#define endl '\n'
#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, start, end) for(int i = start; i <= end; ++i)
#define erp(i, end, start) for(int i = end; i >= start; --i)

using i64 = long long;
using pii = pair<int, int>;
using pll = pair<i64, i64>;

void solve() {
    int n;
    cin >> n;
    string s;
    cin >> s;
    s = '@' + s;
    stack<char> st;
    rep(i, 1, n) {
        if(st.empty()) {
            st.push(s[i]);
        } else {
            char c = st.top();
            if((c == 't' && s[i] == 'b') || (c == 'f' && s[i] == 'c')) st.pop();
            else st.push(s[i]);
        }
    }
    cout << st.size() << endl;
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    solve();
    return 0;
}
```


## **C - tb的路径问题**

### 思路

分奇偶考虑，先考虑偶数，对于大于等于4的偶数4步就能够从$(1,1) \rightarrow (n, n) $，因为$n$与$n -1 $的公因数除了1只有2，证明如下：

$n$和$n-2$为偶数，所以必然存在一个公因子2，假设除了2还存在其他公因数，设这个公因子为$k$,  那么总会有$ (n - 2) \% k = (-2) \% k \neq 0$，所以$n$和$n - 1$只存在一个除1外的因子。

对于大于4的奇数可以先到达$(n - 1, n - 1) $在加上2步到达$(n, n) $

剩余没有讨论的数1、2、3，直接计算即可。


### 代码

```c++
#include <bits/stdc++.h>

using namespace std;

#define endl '\n'
#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, start, end) for(int i = start; i <= end; ++i)
#define erp(i, end, start) for(int i = end; i >= start; --i)

using i64 = long long;
using pii = pair<int, int>;
using pll = pair<i64, i64>;

void solve() {
    int n;
    cin >> n;
    if(n == 1) {
        cout << 0 << endl;
    } else if(n == 2) {
        cout << 2 << endl;
    } else if(n == 3) {
        cout << 4 << endl;
    } else if(n & 1) {
        cout << 6 << endl;
    } else {
        cout << 4 << endl;
    }
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    solve();
    return 0;
}
```


## **D - tb的平方问题**

### 思路

枚举所有的区间并判断，这个区间和（预处理数组的前缀和）是否为完全平方数，如果为完全平方数，给区间中的每一个点加1（使用差分数组处理）。

### 代码

```c++
#include <bits/stdc++.h>

using namespace std;

#define endl '\n'
#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, start, end) for(int i = start; i <= end; ++i)
#define erp(i, end, start) for(int i = end; i >= start; --i)

using i64 = long long;
using pii = pair<int, int>;
using pll = pair<i64, i64>;

void solve() {
    int n, q;
    cin >> n >> q;

    int a[n + 1] = {0};
    rep(i, 1, n) cin >> a[i];

    i64 sum[n + 1] = {0};
    rep(i, 1, n) sum[i] = sum[i - 1] + a[i];

    int b[MAXN] = {0};
    rep(i, 1, n) {
        rep(j, i, n) {
            i64 y = sum[j] - sum[i - 1];
            i64 x = sqrt(y);
            if(x * x == y) {
                b[i]++;
                b[j + 1]--;
            }
        }
    }
    rep(i, 1, MAXN) b[i] += b[i - 1];
    rep(i, 1, q) {
        int x;
        cin >> x;
        cout << b[x] << endl;
    }
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    solve();
    return 0;
}
```