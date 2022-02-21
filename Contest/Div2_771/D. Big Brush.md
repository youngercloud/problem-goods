首先需要说明的是这道题和 `LC Strang Printer II` 的做法不一样，是没有办法用拓扑排序做的，虽然他们的题目很相似。 

那道题的做法中， 我们构建的图是 
``` 
g[curColor].push_back(prevColor)
``` 

然而这道题为了输出所有的操作，如果不把当前的颜色加到边中，那就无法输出最后一步的操作，而如果加进去了，自己将会和自己成环。 

那么我们怎么办呢？实际上还是应用同样的思想，反向解决问题。 

我们可以确定的是，现在图上如果存在一个 `2x2` 的颜色全部都相同的区域，那片区域肯定是最后一次被粉刷的。 
所以这就意味着这片区域不论之前是什么颜色，反正都要被最后这一次 `2x2` 的粉刷覆盖掉 

举例子，`X` 代表不关心
``` 
3 3 X
3 2 2 
X 2 2
``` 
最中间的那个 2，之前很非常有可能是一个 3，只不过最后被后来画的 2 给覆盖掉了，那我们自然可以大胆猜测中间的那个位置是原来是 3。 既然我们猜测的是原来的状态，那么我们可以从现在的状态开始，向过去的状态进行 BFS 搜索 

我们在最开始需要做的，由于是从现在搜到过去，那就把现在的状态，也就是给定的 input 中所有的 2x2 且颜色相同的方格，把他们的起始点和颜色加入到队列中。 

比如我们把这里的 (1, 1, 2)[x, y, color] 扔进队列里，作为 BFS 搜索的开始。

并且现在，我们在 (1, 1, 2) 染了的颜色 2 没有被其他颜色覆盖，我们也可以知道染颜色 2 就是 (1, 1) 这个位置下的最后一笔，所以答案中一定会有 (1, 1, 2)，我们可以现在把 (1, 1, 2) 加入到答案中。 

接下来我们就可以继续向过去的状态搜索了，也就是开始猜测中间的位置原来是个 “3” 了。 

我们从队列到中弹出(1, 1)。针对我们刚刚弹出的 (1, 1) 位置，我们此时需要查看：上，下，左，右，左上，左下，右上，右下，共八个方向。此时我们发现左上的那个颜色是 3。 

同理，我们需要做的仍然是查看我们是否有一个 `2x2` 的方格，并且他们是相同颜色的。我们有 
```
3  3 
3  2 
```
欸？貌似方格内所有颜色并不是相同的？可是别忘了，这个 2 是后来才被染上色的，所以才把 3 个覆盖了。所以在搜索中，我们在向过去的状态探索之前，需要把这样的方块标记为 `-1` 

``` 
3  3 
3 -1 
``` 
所以，在接下来的搜索过程中，我们要查看的其实是 `2x2` 的方格，他们应该是相同颜色的，或者不相同颜色是未来的状态（从未来搜到过去，已被标记为 -1）的。 

好了，我们现在依靠着左上角的 3，找到了合法的颜色为 3 的方格了，我们就可以把 (0, 0, 3) 加入到队列中，然后也把 (0, 0, 3) 加入到答案中。 

就这么一直搜下去，直到队列为空。如果最后矩阵上还有不是 `-1` 的格子，表示不可能，输出 -1。 

如果结果合法，因为我们是向过去的状态搜索，所以我们需要先把结果反转，然后再进行输出。

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

int n, m;
int a[1001][1001];
int visited[1001][1001];

int check(int x, int y) {
	if (x < 0 || y < 0 || x >= n - 1 || y >= m - 1 || visited[x][y]) return -1;
	int color = -1;
	for (int i = 0; i < 2; i++) {
		for (int j = 0; j < 2; j++) {
			if (a[x + i][y + j] != -1) {
				if (color != -1 && a[x + i][y + j] != color) {
					return -1;
				} else {
					color = a[x + i][y + j];
				}
			}
		}
	}
	visited[x][y] = 1;
	return color;
}

void solve() {
	scanf("%d%d", &n, &m);
	rep (i, 0, n) {
		rep (j, 0, m) {
			scanf("%d", &a[i][j]);
		}
	}
	queue<array<int, 3>> q;
	vector<array<int, 3>> ans;
	// last step
	for (int i = 0; i < n - 1; i++) {
		for (int j = 0; j < m - 1; j++) {
			int c = check(i, j);
			if (c != -1) {
				q.push({i, j, c});
			}
		}
	}
	while (!q.empty()) {
		auto [x, y, c] = q.front();
		q.pop();
		
		a[x][y] = a[x + 1][y + 1] = a[x][y + 1] = a[x + 1][y] = -1;
		ans.push_back({x, y, c});
		for (int i = -1; i <= 1; i++) {
			for (int j = -1; j <= 1; j++) {
				int nx = x + i;
				int ny = y + j;
				int c = check(nx, ny);
				if (c != -1) {
					q.push({nx, ny, c});
				}		
			}
		}
	}
	
	rep (i, 0, n) {
		rep (j, 0, m) {
			if (a[i][j] != -1) {
				printf("%d\n", -1);
				return;
			}
		}
	}
	printf("%d\n", ans.size());
	reverse(ans.begin(), ans.end());
	for (auto a : ans) {
		printf("%d %d %d\n", a[0] + 1, a[1] + 1, a[2]);
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

// FAST CP: D:\Codes\problem-goods\playground
```