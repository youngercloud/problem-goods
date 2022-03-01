本题的题意是要找出数组中所有的逆序对(i, j)， 然后让 i 和 j 之间连接做一条无向图的边。最后问这些边作完之后，形成的图中有多少强联通分量。 

首先我们需要知道的是，找出数组中所有逆序对个数的算法，最快也要 NlogN。 

熟悉逆序对算法的也许知道，不论是用 BIT 还是用归并排序。我们得到的都是包含合法逆序对的区间长度，如果我们尝试通过遍历这个长度，去输出每一个逆序对的话，那就是 N^2 的复杂度。再加上我们需要用并查集来确定强连通分量的个数，必然是会超时的。 

那么我们怎么办呢？ 此刻我们必须考虑到速度的特殊性质，目前的数组是一个 Permutation，那么这实际上意味着什么呢？ 

我们来看第 3 个例子 

`6 1 4 2 5 3` 

这个例子的答案是 1， 为什么呢？因为第 1 个元素是 6 啊，这个数组的长度就是 6，目前的数组是一个 Permutation，那么我们必然都可以保证后面的元素一定都比他小，所以后面的元素一定都能和 6 组成逆序对。 

再看第 2 个例子 

`2 1 4 3 5` 

这个例子的答案是 3, 按照上面例子，不难发现，这就是由于 2 1 在一起，4 3 在一起，和 5 单独在一起造成的。 

我们有以下结论: 如果到 i 为止，当前数组的最大值就是 i 的话，那我们就会形成一个新的连通图。 

看第 3 个例子，到 i = 6 的时候，数组的最大值是 6，那就会形成新的（也是唯一的连通图） 

看第 2 个例子： 
- 到 i = 2 的时候，数组的最大值是 2，会形成新的的连通图 
- 到 i = 4 的时候，数组的最大值是 4，会形成新的的连通图 
- 到 i = 5 的时候，数组的最大值是 5，会形成新的的连通图

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
	ll mx = 0, ans = 0;
	rep (i, 0, n) {
		mx = max(mx, arr[i]);
		if (i + 1 == mx) ans++; 
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