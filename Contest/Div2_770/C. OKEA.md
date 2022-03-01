本题构造的方法其实很容易可以从例子中看出来，就是我们要纵向输出 1 2 3... 然后再纵向输出 4 5 6.... 

我们是不可能横向输出的，因为简简单单的横向输出可以很容易把我们搞崩溃，比如说下面这个矩阵 
``` 
1 2 
3 4
``` 
我们发现第 1 行，如果选择的区间长度为 2， 平均数就直接不是整数了，更不要说第一行是 `1 2 3 4 ` 或更长

所以我们的做法是先按照上述方法构造出一个矩阵， 然后查看矩阵元素两两之间能不能算出一个整数平均数，如果都可以的话就输出该矩阵 

那有些人可能会问了，如果矩阵元素两两直接相加数字除以 2 可以的话，那 3 个数字除以 3 一定可以吗？答案是可以的，<del>在比赛时多画几个矩阵确认下就好了</del>

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

ll arr[100005];

void solve() {
	ll n, k;
	cin >> n >> k;
	ll arr[n][k];
	ll c = 1;
	rep (i, 0, k) {
		rep (j, 0, n) {
			arr[j][i] = c++;
		}
	}
	
	rep (i, 0, n) {
		rep (j, 0, k - 1) {
			if ((arr[i][j] + arr[i][j + 1]) % 2 != 0) {
				cout << "NO" << endl;
				return;
			}
		}
	}
	
	cout << "YES" << endl;
	rep (i, 0, n) {
		rep (j, 0, k) {
			cout << arr[i][j] << " ";
		}
		cout << endl;
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
