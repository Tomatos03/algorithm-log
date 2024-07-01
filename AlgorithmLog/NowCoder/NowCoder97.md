# NowCoder-Beginer

## **A - 三角形**

## 思路

记录边的数量，当存在一条边的数量大于3时答案为YES，否则为NO

### 代码

```c++
#include <bits/stdc++.h>

using namespace std;

#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, starts, ends) for(int i = starts; i <= ends; ++i)

typedef long long int64;

void sovle() {
    int n;
    cin >> n;
    int a[n + 1] = {0};
    bool ok = false;
    rep(i, 1, n) {
        int x;
        cin >> x;
        a[x]++;
        ok |= a[x] >= 3;
    }
    cout << (ok ? "YES" : "NO") << endl;
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    sovle();
    return 0;
}
```



## **B - 好数组**

### 思路

$设a_i > a_j, |a_i - a_j| < a_i \times a_j \iff a_i < a_j(1 + a_i)$，显然只要$a_i \neq 0 \wedge \ a_j \neq 0$，必然满足上式

### 代码

```c++
#include <bits/stdc++.h>

using namespace std;

#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, starts, ends) for(int i = starts; i <= ends; ++i)

typedef long long int64;

void sovle() {
    int n;
    cin >> n;
    int a[n + 1] = {0};
    bool zero = false;
    rep(i, 1, n) {
        cin >> a[i];
        zero |= a[i] == 0;
    }
    cout << (zero ? "YES" : "NO") << endl;
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    sovle();
    return 0;
}
```


## **C - 前缀平方和序列**

### 思路

让cnt表示$i * i <= x$的数量，当$cnt < n$答案是0，当$cnt >= n$时，答案为$C_{cnt}^{n}$

最终答案非常大，需要进行取余，能够利用如下性质在计算过程中进行取余

### 性质

$(b \times a) \mod p=(a \times b^{p −2}) \mod p$，a是 b 的倍数且 b 和 p 互质


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
    int cnt = 0;
    for(int64 i = 1; i * i <= x; ++i) {
        ++cnt;
    }
    if(cnt < n) {
        cout << 0 << endl;
        return;
    }
    int k = n, num = cnt;
    int64 a = 1;
    while(k > 0) {
        a = a * num % MOD;
        --num;
        --k;
    }
    int64 base = 1, mx = MOD - 2, b = 1;
    rep(i, 1, n) {
        base = (base * i) % MOD;
    }
    while(mx > 0) {
        if(mx & 1) {
            b = b * base % MOD;
        } 
        base = base * base % MOD;
        mx >>= 1;
    }
    int64 ans = ((a % MOD) * (b % MOD)) % MOD;
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


## **D - 走一个大整数迷宫**

### 思路

观察到$p^{2^{b_{i, j}}} \% (p - 1) == 1$，每一轮相当于加上$a_{i, j}$，让$dp[i][j][k]$表示第$i$行第$j$列余数为$k$时的最短时间，对给定数组$a$进行bfs更新$dp[i][j][k]$，算法时间复杂度为$O(nmp)$

### 代码

```c++
#include <bits/stdc++.h>

using namespace std;

#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, starts, ends) for(int i = starts; i <= ends; ++i)

typedef long long int64;

struct node{
    int x, y, z;
};

int dp[11][11][10011];
int dx[] = {0, 0, -1, 1}, dy[] = {1, -1, 0, 0};

void sovle() {
    int n, m, p;
    cin >> n >> m >> p;
    p--;
    int a[n + 1][m + 1] = {0}, b[n + 1][m + 1] = {0};
    rep(i, 1, n) {
        rep(j, 1, m) {
            int x;
            cin >> x;
            a[i][j] = x % p;
        }
    }
    rep(i, 1, n) {
        rep(j, 1,m) {
            cin >> b[i][j];
        }
    }
    if(a[1][1] == 0 && n == 1 && m == 1) {
        cout << 0 << endl;
        return;
    }

    queue<node> que;
    que.push({1, 1, a[1][1]});

    while(!que.empty()) {
        node t = que.front();
        que.pop(); 
        rep(i, 0, 3) {
            int nx = dx[i] + t.x, ny = dy[i] + t.y, nz = (t.z + a[nx][ny]) % p;
            if(nx < 1 || ny < 1 || nx > n || ny > m || dp[nx][ny][nz] != 0) continue;
            dp[nx][ny][nz] = dp[t.x][t.y][t.z] + 1;
            que.push({nx, ny, nz}); 
        }
    }
    cout << (dp[n][m][0] == 0 ? -1 : dp[n][m][0]) << endl;
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    sovle();
    return 0;
}
```