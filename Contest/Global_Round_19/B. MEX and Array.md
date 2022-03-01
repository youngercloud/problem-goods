让我们来复述一下本题的题意，针对一个数组的所有 Subarray，我们希望将这些 Subarray 分段(Partition)

分段后我们会得到一些分数，得到的分数是分段(Partition)的个数 再加上每个分段数的 "First-Missing Non-negative number" 

这道题的数据范围很小，所以很自然的我们可以枚举所有的子数组 ，接下来就是如何算一个子数组的分数了。 

接下来就是一个很重要的发现了，实际上给定一个子数组， 在所有 Partition 的方案中，最优策略一定是把他们全都给分开。

这是为什么呢? 实际上是基于一种贪心的思想，就是说如果我们只要分割出了一个区间，我们的分数就可以涨 1，因为我们的分段数提升了。 

有些策略可能想着不把数组分开，最后把所有的 "First-Missing Non-negative number" 凑合在一起最后来一个大的数字。 

针对这种情况，我们来看一个例子 

```
[0, 1, 2, 3, 4, 5] -> 1(Partition) + 6(Missing) = 7 

[0], [1], [2], [3], [4], [5] = 6(Partition) + 1(Missing, from first zero) = 7 
```
上方的例子显而易见， 就算是在 "First-Missing Non-negative number" 策略最优的情况下， 依然是和把它们全部分开得到的分数相等。 

所以我们的做法是，查看一个子数组能够被分成多少个长度为 1 的区间，将分数加上这些区间的个数，
然后查看所有这个子数组中的元素有多少个 0 即可，每遇见一个 0 我们的分数就加一。 

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
	int ret = 0;
	for (int i = 0; i < n; i++) {
		for (int j = i; j < n; j++) {
			int sum = (j - i + 1);
			for (int k = i; k <= j; k++) sum += (arr[k] == 0);
			ret += sum;
		}
	}
	cout << ret << endl;
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
