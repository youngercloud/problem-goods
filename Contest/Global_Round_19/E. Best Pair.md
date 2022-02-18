本题实际上就是暴力。但是关键是在怎么暴力 

首先一个很直观的想法就是说我们要保证我们的 values 要有序， 
这样如果我们是从大到小遍历的话，找到了更大的合法解，就不用查看更小的 values 了。

那么针对 cnt 和 value，我们先遍历哪个呢？自然是选择遍历 cnt。 cnt 的数量自然比 value 的数量要少很多，因为值相同的元素都被算到一个 cnt 了。 

另外，如果我们使用 Map 对 cnt 计数并排序，假设我们的 cnt 是 [1, 2, 4]，代表某些元素出现了一次两次和四次 

我们需要 cntx 和 cnty， 如果我们将在之后遍历 cntx = 4 和 cnty = 1 ，那么当前自然没有必要遍历 cntx = 1 和 cnty = 4，
因为这两者实际上指向的都是同两个 cnt，就是 1 和 4.

所以，我们只需遍历所有的 cntx，发现 cnty 比 cntx 大的时候 break 即可。

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


void solve() {
	int n, m;
	cin >> n >> m;
	map<int, int> cnt;
	set<array<int, 2>> bad;
	
	rep (i, 0, n) {
		int v; cin >> v;
		cnt[v]++;
	}
	
	rep (i, 0, m) {
		int a, b; cin >> a >> b;
		bad.insert({a, b});
		bad.insert({b, a});
	}
	
	map<int, vector<int>, greater<>> cnt2val;
	for (auto& [v, c] : cnt) {
		cnt2val[c].push_back(v);
	}
	
	ll res = 0;
	for (auto& [c1, v1] : cnt2val) {
		for (auto& [c2, v2] : cnt2val) {
			if (c2 < c1) break;
			ll sum = c1 + c2;
			for (auto& a : v1) {
				for (auto& b : v2) {
					if (a != b && bad.find({a, b}) == bad.end()) {
						res = max(res, sum * (a + b));
						break;
					}
				}
			}
		}
	}
	cout << res << endl;
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