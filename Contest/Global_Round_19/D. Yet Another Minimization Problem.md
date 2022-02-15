首先两个事情需要说明，这样的题目请务必想到
- 这个数据范围，想到不可能通过暴力 DFS 来找到答案之后，请务必想到 DP。特别的是，
  值小并且数据又小，非常有可能是背包 DP
- `(x + y)^2` 请务必想到 `x^2 + 2xy + y^2`

我们将公式拆分之后可以看到，在这里 `x^2` 和 `y^2` 是常数，
最小化 `2xy` 就可以了

但是 2 也是常数，所以我们真正需要最小化的实际上是 `xy`

我们看一个例子

`[2, 5, 6]`

如果我们正在跑着 DP 呢，现在有一个新值 X，那么会发生什么？

式子会变成，因为前面的每个元素都要和它结合。
```
(2 + X)^2 + (5 + X)^2 + (6 + X^2)
```

由于前面我们说过，我们只关心 `xy`，式子化简为
```
2X + 5X + 6X
```

把 `X` 提出来

```
X * (2 + 5 + 6)
```

这个时候，我们就应该知道状态转移如何进行了。我们从右往左看，针对一个当前数字 `a`，
从左往右看假设当前的和为 `sum`，那么我们新状态的值就是 `a * sum`

由于我们此时关心两个数组，我们假设上面的那个数组是第一个数组，
那个从左往右看的和为 `top`

设第二个数组的数字是 `b`，那么第二个数组，新状态的值就是 `b * bottom`

两者最后需要加和，即
```
a * top + b * bottom
```

当然，`a` 和 `b` 还可以进行交换，也就是
```
b * top + a * bottom
```

我们使用背包 DP 的模板，每次我们可以选择交换 `a` 和 `b` 或者不交换 `a` 和 `b`。

我们遍历的容量是 `topsum`，然后通过前缀和就可以知道 `bottomsum`，降低了复杂度

```
typedef long long ll;
#include <bits/stdc++.h>

using namespace std;

#define rep(i, a, b) for (int i = (a); i < (b); i++)
#define all(cont) cont.begin(), cont.end()
#define ms(a) memset(a, 0, sizeof(a))
#define EPS 1e-9
	
template<class T> void chmax(T & a, const T & b) { a = max(a, b); } 
template<class T> void chmin(T & a, const T & b) { a = min(a, b); } 
template <class T> void oa(const vector<T>& a) { for (int i = 0; i < a.size(); ++i) cout << a[i] << " \n"[i + 1 == a.size()]; }
typedef long long ll;
ll MOD = 1e9 + 7;;

void add(ll &x, ll a) {
	x += a;
	if (x >= MOD) x -= MOD;
}
void sub(ll& x, ll a) {
	x = ((x - a) % MOD + MOD) % MOD;
}

ll arr1[105];
ll arr2[105];
ll ttsum[105];

void solve() {
	ll n;
	cin >> n;
	rep (i, 0, n) cin >> arr1[i];
	rep (i, 0, n) cin >> arr2[i];
	ttsum[0] = arr1[0] + arr2[0];
	rep (i, 1, n) ttsum[i] = ttsum[i - 1] + arr1[i] + arr2[i];
	vector<ll> dp(10005, 1e18);
	dp[0] = 0;
	for (int i = 0; i < n; i++) {
		vector<ll> newdp(10005, 1e18);
		for (int top = 0; top <= 10000; top++) {
			int a = arr1[i];
			int b = arr2[i];
			int btm = (i == 0 ? 0 : ttsum[i - 1]) - top;
			if (top + a <= 10000 && dp[top] != 1e18) {
				newdp[top + a] = min(newdp[top + a], dp[top] + top * a + btm * b);
			}
 			if (top + b <= 10000 && dp[top] != 1e18) {
				newdp[top + b] = min(newdp[top + b], dp[top] + top * b + btm * a);
			}
		}
		dp = newdp;
	}
	ll ans = 1e18;
	for (int i = 0; i <= 10000; i++) {
		ans = min(ans, dp[i]);
	}
	ans *= 2;
	rep (i, 0, n) {
		ans += (arr1[i] * arr1[i] + arr2[i] * arr2[i]) * (n - 1);
	}
	cout << ans << endl;
}

	
int main() {
	ios_base::sync_with_stdio(0);
	cin.tie(nullptr);
	
	int q;
	cin >> q;
	while (q--)
		solve();
	
}

// FAST CP: D:\Codes\problem-goods\playground
```