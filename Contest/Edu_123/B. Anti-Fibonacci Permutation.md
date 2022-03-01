可能与大多数人的想法不同，这里复盘下我在比赛时的想法

先构造一个看似最不可能的序列，例如当 k = 5 时，我们可以构造这样的数组
```
5 4 3 2 1
```

接着，我们可以把将数组 “向左滚动”，也就是产生下面的序列
```
5 4 3 2 1
4 3 2 1 5
3 2 1 5 4
2 1 5 4 3 
1 5 4 3 2
```

我们发现上面产生的 5 个数组都是符合条件的，然后我就交了

嗯，然后就 `Wrong A at test 1`

寻找问题，我们考虑下 k = 3 会怎样
```
3 2 1
2 1 3
...
```
很明显 `2 1 3` 在这里是不符合条件的，因为 `2 + 1 = 3`

此时我想到的是，立即再想一下 k = 6 和 k = 7 是什么的情况，之后发现 6 和 7
在上方的策略下，输出的都是满足条件的数组。

所以，我基本断定 `k = 3` 在这里是一个特例，特判即可，最终 AC

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
	ll n;
	cin >> n;
	vector<int> a(n);
	int cnt = n;
	for (int i = 0; i < n; i++) {
		a[i] = cnt--;
	}	
	if (n == 3) {
		vector<int> a1 = {3, 2, 1};
		vector<int> a2 = {1, 3, 2};
		vector<int> a3 = {3, 1, 2};
		oa(a1);
		oa(a2);
		oa(a3);
		return;
	}
	rep (_, 0, n) {
		oa(a);
		int first = a[0];
		rep (i, 0, n - 1) {
			a[i] = a[i + 1];
		}
		a[n - 1] = first;
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
