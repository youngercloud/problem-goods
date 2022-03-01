和官方题解的思路一样，由于数据范围是` n = 5000`，所以我们可以用 `O(N^2)` 的解法。

我们要在数组中选择 `k` 个元素，然后将这些元素的值提升 `x`

由于 `k` 是个正数，所以当然我们永远都希望执行这个操作。关键是把 `x` 加到数组的哪些元素上。

答案是，我们每次都希望对一个数组的一个 Subarray 上的元素增加 `x`，换句话说，对连续的区间上的元素都增加 `x`。

因为我们的想要的答案是 "Subarray" 的和的最大值。显然我们不会从整个数组中随便挑元素增加 x。

那么我们就可以枚举 Subarray 的长度 ，一个 Subarray 在某个长度下他们的和的最大值是多少。
我们记 `dp[len]` 是长度所有为 `len` 的 Subarray 中，Subarray 元素总和的最大值。

接下来我们有 2 种情况
1. 选择元素的个数 `k` 小于等于 Subarray 的长度 `len`，表明我们仅有 `k` 次机会提升值。
- 答案为 `ret[k] = dp[len] + (k * x)`
2. 选择元素的个数 `k` 大于等于 Subarray 的长度 `len`，我们也只能执行操作到 `len` 个元素上
- 答案为 `ret[k] = dp[len] + (len * x)`

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
ll mod = 1e9 + 7;

void solve() {
	ll n, x;
	cin >> n >> x;
	vector<ll> arr(n);
	vector<ll> presum(n);
	rep (i, 0, n) cin >> arr[i];
	presum[0] = arr[0];
	rep (i, 1, n) {
		presum[i] = presum[i - 1] + arr[i];
	}
	
	vector<ll> dp(n + 1, -1e17);	// max sum value by given length
	dp[0] = 0;
	
	for (int i = 0; i < n; i++) {
		for (int j = i; j < n; j++) {
			int len = j - i + 1;
			dp[len] = max(dp[len], presum[j] - (i == 0 ? 0 : presum[i - 1]));
		}
	}
	
	vector<ll> ret(n + 1);
	for (int k = 0; k <= n; k++) {
		for (int len = 0; len <= n; len++) {
			if (k <= len) {
				ret[k] = max(ret[k], dp[len] + k * x);
			} else {
				ret[k] = max(ret[k], dp[len] + len * x);
			}
		}
	}
	oa(ret);
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