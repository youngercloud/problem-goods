这题很重要的一点，就是说我们为了字典序最小，总是想要把 1 提到前面来。

我们总是能通过

- 选择数组中的第 1 个数作为翻转的开头位置
- 选择数组中的数字 1 的位置作为翻转的结尾

这样数字 1 就到了数组开头的位置，字典序就最低了


当然我们很快就可以想到另外一种情况

```
1 2 3 5 4
```

这个时候 `1` 已经在最前面了

这个时候最优策略肯定是反转 `5 4`，让数组成为 `1 2 3 4 5`

所以我们的策略就可以 "退化" 成

- 选择数组中的第 1 个破坏递增的数，作为翻转的开头位置
- 选择数组中的 破坏递增的数 的位置 作为翻转的结尾

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
	int n;
	cin >> n;
	vector<ll> arr(n);
	rep (i, 0, n) cin >> arr[i];
	ll l = -1, r = -1;
	for (int i = 0; i < n; i++) {
		if (arr[i] != i + 1) {
			l = i;
			break;
		}
	}

	for (int i = 0; i < n; i++) {
		if (arr[i] == l + 1) {
			r = i;
			break;
		}
	}

	if (l != -1) {
		reverse(arr.begin() + l, arr.begin() + r + 1);
	} 
	oa(arr);
}

	
int main() {
	ios_base::sync_with_stdio(0);
	cin.tie(nullptr);
	
	int q;
	cin >> q;
	while (q--)
		solve();
	
}
```