这道题实际上并不是那么好想， 这里需要关心的是为什么 Bob 是 d + 3 

因为我们可以看到 3 在这里是一个奇数，有性质 
- 偶数加奇数必定等于奇数 
- 奇数加奇数必定等于偶数 

这有什么意义呢？我们可以看到题中所给的两种操作，一个是加法操作，一个是 XOR 操作。

加法操作的性质我们已经讲到， 然而大多数人不知道的是，XOR 操作实际上也具有相同的性质，也就是 
- 偶数 XOR 奇数必定等于奇数 
- 奇数 XOR 奇数必定等于偶数 

所以我们可以检查这样的答案，我们从给定的 X 开始， 将数组中的元素全部和 X 执行 XOR 操作 

如果发现 X 执行 XOR 最后的结果和 Y 奇偶性是相同的，那么我们就可以输出 YES

因为 Bob 是 d + 3，所以一旦 X 执行 XOR 最后的结果和 Y 奇偶性不相同， 那就说明 Bob 最终可以获得他想要的答案。

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
	ll n, x, y, v;
	cin >> n >> x >> y;
	ll arr[n];
	rep (i, 0, n) cin >> arr[i];
	rep (i, 0, n) x ^= arr[i];
	if (x % 2 == y % 2) {
		cout << "Alice" << endl;
	} else {
		cout << "Bob" << endl;
	}
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