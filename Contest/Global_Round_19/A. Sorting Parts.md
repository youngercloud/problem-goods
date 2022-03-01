所有的数据范围最大是 100 * 1e4, 本题时间限制为一秒，
但是 Codeforces 一秒钟可以跑 1e9 的数据，所以本题可以暴力模拟。

另外的一个更简单的解法是，我们可以直接看看数组是否已经排完序，
如果已经排完序则输出 NO， 否则输出 YES。

其实比较好想，如果一个没有排序的数组，
我们在题中选择任意的长度，都能把它变为有序吗？答案显然是否定的。

这里给出暴力解法
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
	vector<int> arr(n);
	rep (i, 0, n) cin >> arr[i];
	
	for (int len = 1; len <= n - 1; len++) {
		auto b = arr;
		sort(b.begin(), b.begin() + len);
		sort(b.begin() + len + 1, b.end());
		for (int i = 1; i < b.size(); i++) {
			if (b[i - 1] > b[i]) {
				cout << "YES\n";
				return;
			}
		}
	}
	cout << "NO\n";
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

