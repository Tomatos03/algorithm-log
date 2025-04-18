# Educational Codeforces Round 167 Div. 2

## A. Catch the Coin

### 思路

对于$y >= -1$总存在一种方案：先通过斜向移动调整水平位置与银币位置一直，此时y的相对距离一直不变，又由于当前位置在硬币的正下方，所以一定能够获得该硬币

### 代码

```c++
#include <bits/stdc++.h>

using namespace std;

#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, starts, ends) for(int i = starts; i <= ends; ++i)

typedef long long int64;

void sovle() {
    int x, y;    
    cin >> x >> y;
    cout << (y >= -1 ? "Yes" : "No") << endl;
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



## B. Substring and Subsequence
### 思路

先取a作为最终字符串c，由于字符串b是字符串c的子序列，c需要尽量多的包含b字符串的字符且保持相对顺序不变。数据给出的a、b较小考虑直接枚举

### 代码

```c++
#include <bits/stdc++.h>

using namespace std;

#define MAXN int(2e5 + 10)
#define MOD int(1e9 + 7)
#define rep(i, starts, ends) for(int i = starts; i <= ends; ++i)

typedef long long int64;

void sovle() {
    string a, b;
    cin >> a >> b;
    int alen = a.size(), blen = b.size();
    a = '@' + a, b = '@' + b;
    int len = alen + blen;
    int ans = alen + blen;
    rep(i, 1, blen) {
        int k = i, cnt = 0;
        rep(j, 1, alen) {
            if(a[j] == b[k]) {
                ++k;
                ++cnt;
            }
        }
        ans = min(ans, len - cnt);
    }
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



## C. Two Movies
### 思路

让$lsum$表示第一个电影的评分，$rsum$表示第二个电影的评分，如果$a[i] \neq b[i]$，那么取值大于等于0的一方的分值加到对应的电影评分和中，如果$a[i] = b[i]$，记录正数和负数的数量。对于$lsum、rsum$中分数较高的一者降低对应分数，对于分数较低的一者，提高其分数。

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
    int a[n + 1] = {0}, b[n + 1] = {0};
    rep(i, 1, n) cin >> a[i];
    rep(i, 1, n) cin >> b[i];
    int lsum = 0, rsum = 0;
    int pos = 0, neg = 0;
    rep(i, 1, n) {
        if(a[i] == b[i]) {
            pos += a[i] > 0 ? 1 : 0;
            neg += a[i] < 0 ? 1 : 0;
        } else if(b[i] > 0) {
            rsum += b[i];
        } else if(a[i] > 0){
            lsum += a[i];
        }
    }
    while(neg > 0) {
        if(lsum >= rsum) --lsum;
        else --rsum;
        --neg;
    }
    while(pos > 0) {
        if(lsum >= rsum) ++rsum;
        else ++lsum;
        --pos;
    }
    cout << min(lsum, rsum) << endl;
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

