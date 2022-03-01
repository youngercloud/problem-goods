事实上本题在例子中所有的情况就已经说明了什么情况下是需要输出 -1 的 

- 数组长度为 3，数字为奇数
- 数组中间的所有数字全部都为 1 

至于其他的情况都是可行的，这里可以自己多画画，例如
``` 
-> 5 1 2 1 5 
-> 6 2 0 1 5 
-> 7 0 0 2 5 
-> 9 0 0 0 5
``` 
最后我们需要关心的是，在可行的情况下，我们需要多少次操作，实际上不难想到：

- 针对一个偶数， 操作的次数就是 `nums[i] / 2`
- 针对一个奇数， 我们必定要找到一个中间的其他数字将其变成偶数，
  所以我们必须要花费额外的一次操作在中间进行移动，
  操作的次数就是 `1 + (nums[i] / 2)`




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
	vector<ll> arr(n);
	ll cnt1 = 0;
	rep (i, 0, n){ 
		cin >> arr[i];
		if (i >= 1 && i < n - 1 && arr[i] == 1) cnt1++;
	}
	if (n == 3 && (arr[1] & 1) || cnt1 == n - 2) {
		cout << -1 << endl;
		return;
	}
	
	ll sum = 0;
	for (int i = 1; i < n - 1; i++) {
		if (arr[i] & 1) {
			sum += 1 + (arr[i] / 2);
		} else {
			sum += (arr[i] / 2);
		}
	}
	cout << sum << endl;
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