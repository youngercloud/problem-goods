这道题就算是签到题了吧，应该也没什么可写的

如果一个门在钥匙之前出现，则打印 `NO`

如果上述情况没有出现，打印 `YES`

```
typedef long long ll;
#include <bits/stdc++.h>

using namespace std;

#define rep(i, a, b) for (int i = (a); i < (b); i++)
#define all(cont) cont.begin(), cont.end()
#define ms(a) memset(a, 0, sizeof(a))
#define EPS 1e-9
	
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
	string s;
	cin >> s;
	int r = 0, g = 0, b = 0;
	int ok = 1;
	rep (i, 0, s.size()) {
		if (s[i] == 'R' && !r) ok = 0;
		if (s[i] == 'G' && !g) ok = 0;
		if (s[i] == 'B' && !b) ok = 0;
		if (s[i] == 'r') r = 1;
		if (s[i] == 'g') g = 1;
		if (s[i] == 'b') b = 1;
	}
	cout << (ok ? "YES" : "NO") << endl;
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