首先我们能够很容易确定，我们把某个数字在各个数组中的出现的频次相加，必须确保这个频次是偶数，如果有一个数字出现的此时是奇数的的话，那他最终究无法被等分到左右两个子集中。 

现在所有的数字都出现了偶数次，这道题的关键，就是是否可以从这一点，能够想到欧拉回路。 

在一个无向图当中如果所有的点的度数都是偶数，那么一定存在一条欧拉回路。 

那么现在的问题变成了我们如何构造出这一个图呢？ 

本题的第二大关键点，其实就能够想到我们图的边，其实就是每个出现值的位置。 

因为每个值都出现了偶数次，所以他们的出现的位置也出现了偶数次。 

接下来我们当然需要遍历这个图，也就是使用 Hierholzer 算法，
目前为止我们只知道了边的出现次数，但是边互相之间是怎么连接的呢？ 

我们先来看一下这个算法最朴素的模板

``` 
void DFS(string start)
    {
        while (Map[start].size()>0)
        {
            string nextStart = Map[start].front();
            Map[start].pop();
            DFS(nextStart, path);
        }
    } 
```

start 在这里是图节点，但是此次回头看看我们的题目本身，
我们是否需要对算法进行一些改动呢？ 

答案是肯定的，因为我们是需要把一个数组中的元素分为左右两个子集，
所以我们在指定算法中的 next 的时候，并不是 `Map[node].front()` 

我们图的边，其实就是每个出现值的位置。
所以我们可以通过 Map 中弹出的元素来确定这个元素的位置，接下来就是关键。

针对边的连接方法，我们可以寻找与其位置对称的位置作为 next，并且把当前弹出的位置作为 left 子集的一部分，next 作为 right 子集的一部分。 

举例来说，例子中第三个数组长度为 6，所以值为 1 的{3, 1}, 和值为 3 的{3, 4}是对称的，那么图就会是这样的。
```  
|----->  
1     3
<-----|
``` 

因为我们不需要实际把这个图构建出来，只是利用了数组元素的对称，
上面的过程实际上等同于我们连接了一些虚边。

但是很明显，我们所有创造的虚边，绝对能构成一个欧拉回路。
因为我们每次加虚边实际上都是加了两条(即偶数条)边（1 -> 3 和 3 -> 1）。然后我们可以选择把 1 分到左子集，再把 3 分到右子集。 所以针对所有的值，最走一遍欧拉回路，最后的子集分配结果就是答案。

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

vector<ll> arr[200005];
string ans[200005];
map<int, vector<array<int, 2>>> val2pos;

void dfs(int val) {
    while (val2pos[val].size() > 0) {
        auto [r, c] = val2pos[val].back();
        if (!arr[r][c]) {
            val2pos[val].pop_back();
        } else {
            int pre_c = arr[r].size() - 1 - c;
			int nxt = arr[r][pre_c];
            ans[r][c] = 'L';
            ans[r][pre_c] = 'R';
            arr[r][c] = 0, arr[r][pre_c] = 0;
            dfs(nxt);
        }
    }
}


void solve() {
	ll n; cin >> n;
	rep (i, 0, n) {
		ll m; cin >> m;
		ans[i].resize(m);
		arr[i].resize(m);
		rep (j, 0, m) {
			ll v; cin >> v;
			arr[i][j] = v;
			val2pos[v].push_back({i, j});
		}
	}
	
	for (auto& p : val2pos) {
        if (p.second.size() & 1) {
            cout << "NO\n";
            return;
        }
    }
	
	for (auto& [val, pos]: val2pos) dfs(val);
	
	cout << "YES\n";    
    rep (i, 0, n) {
		rep (j, 0, ans[i].size()) {
			cout << ans[i][j];
		}
		cout << endl;
	}
	
}

	
int main() {
	ios_base::sync_with_stdio(0);
	cin.tie(nullptr);
	
	// int q;
	// cin >> q;
	// while (q--)
		solve();
	
}
```
