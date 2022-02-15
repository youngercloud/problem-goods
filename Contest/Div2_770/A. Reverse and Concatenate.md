我们需要考虑这道题的实际意义是什么。 

给定的两个操作的意义，一定是把一个字符串转变为一个回文字符串。 

但是这道题就明确规定，我们只能执行 k 次操作。 所以我们不论怎么翻转，执行完 k 次操作之后我们只可能有两个情况。 

1. 像例子中的那样，获得两个回文字符串 
2. 第3个例子中的那样，原本他就是一个回文字符串，反转之后期仍然是回文字符串 

另外一个情况是我们的K等于0，所以说直接输出例子 4 中的 1 即可 

在很多情况下第1题都是能在例子中看到全部的答案的，这一点需要多加注意。


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
	int n, k;
	string s;
	cin >> n >> k >> s;
	string rs = s;
	reverse(rs.begin(), rs.end());
	if(s == rs || k == 0){
		cout << 1 << endl;
	} else {
		cout << 2 << endl;
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