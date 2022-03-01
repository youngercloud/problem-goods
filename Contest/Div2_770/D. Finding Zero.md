[写的很好的题解](https://blog.csdn.net/m0_51499184/article/details/122803807)

首先要有的 sense 是，我们选择数组中的一个数，再次选择数组中的另外一个数字，这两个数字的差值如果越来越大的话，那么最后导致差值最大的那个，必定是数组中的最小值或者是最大值。 

我们举个例子来说明 

数组 
```
2 4 1 3 5
``` 

假设现在我们选第一个数字 2

接下来我们有 

``` 
4 - 2 = 2
2 - 1 = 1 
3 - 2 = 1 
5 - 2 = 3
``` 
这里面差值最大的数字是三，所以我们能够找到 5 为最大值。 

所以如果让我们选择三个下标 i j k 的话， 我们可以设定初始值 
```
i = 1 
j = 2 
k = 3 
``` 
然后固定` i j `不动， 把 `k` 和数组中的其他元素进行比较，这样 `k` 最后一定指向一个最大值或者最小值

假设这次我们用 `k` 找到的是最大值下标，那么我们可以选择另外的一个元素 `i` 或者 `j`。假设我们用的是 `j`，用上述同样的方法和数组中的其他元素进行比较， 
由于此时最大值的下标我们已经找到了，所以我们再次寻找的时候，如果发现了最大值的下标可以直接进行跳过。到最后 `j` 一定是指向的数组中的最小值。 

现在我们有了最大值的下标 `k` 和最小值的下标 `j` 了，我们发现答案只需要让我们输出两个下标，所以现在就变成了三个下标当中选择两个的问题。 

此时我们需要做的就是另外选择一个新的下标，然后将这个新的下标和三个原来的下标分别进行替换，分别再进行三次查询

如果发现查询结果不变，就说明我们选择的那个下标，就是对结果根本无关紧要的下标，进而我们就找到了会影响结果的那两个下标，输出那两个下标即可



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

int ask(int a, int b, int c) {
	cout << "? " << a << " " << b << " " << c << endl;
	fflush(stdout);
	int ret;
	cin >> ret;
	return ret;
}

void solve() {
	int n;
	cin >> n;
	int a = 1;
	int b = 2;
	int c = 3;
	int res = ask(a, b, c);
	int asked3 = 3;
	for (int i = 4; i <= n; i++) {
		int r = ask(a, b, i);
		if (r >= res) {
			res = r;
			asked3 = i;
		}
	}
	int asked2 = 2;
	for (int i = 3; i <= n; i++) {
		if (i == asked3) continue;
		int r = ask(a, i, asked3);
		if (r >= res) {
			res = r;
			asked2 = i;
		}
	}
	int last = 1;
	while (last == a || last == asked2 || last == asked3) last++;
	
	if (ask(last, asked2, asked3) == res) {
		cout << "! " << asked2 << " " << asked3 << endl;
		return;
	}
	if (ask(a, last, asked3) == res) {
		cout << "! " << a << " " << asked3 << endl;
		return;
	}
	if (ask(a, asked2, last) == res) {
		cout << "! " << a << " " << asked2 << endl;
		return;
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