首先来讲一下什么是珂朵莉树，以这个世界上最幸福的女孩命名的树，诞生于 CF869C

这种数据结构建立在线段树或者树状数组之上，支持： 
- 区间加
- 区间赋值
- 区间第 K 大数字 
- 区间所有元素做 K 次方后求和。 

我们用 Map 来表示这种数据结构，其中 Key 代表着一个区间的左端点，Value 代表着一个区间的值。 

那么区间的右端点在哪里呢？ 此时不要忘记 Map 中的 Key 是有序的，假设当前元素是 `i->first`，那么他的右端点就是 `next(i)->first`

同时，右端点 `next(i)->first` 也可以作为下个区间的左端点。如此反复，每一个元素和下一个元素，都一起代表了一个左闭右开区间。 

所以我们应该这样初始化这个树 
```
Map[1] = 1
Map[n + 1] = 0
``` 
我们来看一下这里的值 1 和 0 是什么呢？实际上就是我们需要存的颜色了，题干表明最开始所有元素都是 1，所以左闭右开区间 [1, n + 1) 的值为 1。而 Map[n + 1] 作为一个虚拟的右端点存在，所以赋值颜色为 0。

这里存了颜色，那么每个元素的值 `ai` 怎么办？别忘了珂朵莉树是建立在线段树或者树状数组上的，这里就需要用到树状数组了。 

比如说我们在 `Map` 上有区间 `[3, 8]`，现在我们需要给 `[5, 8]` 内的所有元素全部加 3 

1. 我们在 Map 上二分搜索找 `5`，发现找不到，只能找到 `3`
2. 假设 `[3, 8]` 这个区间的值是 `4`，那么我们执行赋值操作 `Map[5] = 4` 
3. 于是，我们就有了 `[5, 8]` 这个区间了，左端点 `5` 是刚刚加的，右端点 `8` 是原来就有的。 
4. 区间 `[5, 8]` 要执行加 `3` 操作，最后区间 `[5, 8]` 值为 `3 + 4 = 7`，这就靠线段树或者树状数组实现。 

前三步操作对应珂朵莉树的 `split` 函数，最后一步对应珂朵莉树的 `assign` 函数，代码 
``` 
struct Fenwick {
    vector<ll> f;
    int n;
    using ll = long long;
	
    void init(int len) {
        n = len;
        f.resize(n + 1);
        memset(f.data(), 0, sizeof(ll) * (n + 1));
    }

    void add(int x, ll v) {
        for	(; x; x -= x & -x) f[x] += v;
    }

    void add(int x, int y, ll v) {
        add(y, v);
        if (x > 1) add( x - 1, -v);
    }

    ll query(int x) {
        ll res = 0;
		if (x <= 0 || x > n) return 0;
        for	(; x <= n; x += x & -x) res += f[x];
        return res;
    }
}bit;

void split(map<int, int>& Map, int pos) {
	auto iter = Map.lower_bound(pos);
	if (iter->first != pos) Map[pos] = prev(iter)->second;
}

void assign(map<int, int>& Map, int l, int r, int color) {
	auto it_l = Map.find(l);
	auto it_r = Map.find(r);
	for (auto i = it_l; i != it_r; i++) {
           bit.add(i->first, next(i)->first - 1, ?????????????????????)
        }
	Map.erase(it_l, it_r);	// delete all between two iterator
        Map[l] = color;  // update color
}
``` 

看到上面的代码，既然我们已经能够维护 Color 操作了，也就是说维护区间的颜色，
那么我们如何维护某个元素所对应的值呢？我们之前说过，我们要用树状数组维护这些值。但是我们此时会遭遇到什么问题呢？ 

举例子

1. 比如说一个节点，其颜色为红色，而目前带有红色的节点的值都为 5

2. 现在我们要把其染为绿色，目前带有绿色的节点的值都为 2 

3. 现在该节点为绿色了，那么节点值呢？是 2 嘛？ 

4. 不，应该仍然保持为 5。 

为什么呢？因为我们的操作要么是给区间改颜色，要么是给带某种颜色的区间加上值，这个操作应该不影响节点值本身。

我知道上面说的肯定不明白，换句话说。。这里颜色，对于某区间里的一个节点，只是这个节点的一个”外套“而已

我们把节点通过颜色进行分类，就可以通过颜色来快速找到一些节点，给他们进行加值。

然而这个节点的外套如果换了，也就是从红色外套换为绿色外套，那么这个节点，它真正的值需要换嘛？肯定是不需要的。

一个螃蟹的一生也许会换很多个壳，但是那只螃蟹仍然是那只螃蟹。 

那么我们怎么办呢？ 

- 我们可以用一个数组记录每个颜色所对应的价值，每个颜色每次被加的值，即 `arr[color] += value`，而暂时不关心这些颜色在哪个节点上。 

为什么不需要关心？我们刚才把颜色比做外套，外套有便宜的有贵的。那么不管外套穿在哪个人的身上，外套多少钱是不会跟着穿他的人的变化而变化的。  

- 从上方中的代码我们可以得知，我们每次对”线段树/树状数组“进行的操作都是在改颜色的时候，所以此时我们”线段树/树状数组“里应该记录的是节点值和真实值的差值。 

记录差值？再举例子。。老例子 

比如说一个节点，其颜色为红色，而目前带有红色的节点的值都为 5，就是 `arr[red] = 5`

现在我们要把该节点染色为绿色，带有绿色的节点的值都为 2，就是 `arr[green] = 2`

好了，现在这个节点值是绿色，如果我们直接通过珂朵莉树去查询这个节点的值，那这个节点就是绿色，返回值就是 2。然而，这个节点是 5。 

所以我们在更新这个节点的颜色时，我们要在这个”线段树/树状数组“ 里记录下差值，也就是 `5 - 2 = 3`，计算方法是 `arr[old_color] - arr[new_color]` 

也就是说，上方代码的一大串问号其实就是 

``` 
for (auto i = it_l; i != it_r; i++) {
    bit.add(i->first, next(i)->first - 1, arr[i->second] - arr[color])
}
```

这样，以后我们再进行查询操作时，返回的就是线段树里的差值 + 颜色本身的值，即 `3 + 2 = 5`，就是这个节点的真实值了。 

这也就表明了 `Query` 操作的写法是这样的了
``` 
int i; cin >> i;
auto iter = node2color.lower_bound(i);
if (iter->first > i) iter--;
cout << bit.query(i) + arr[iter->second] << endl;  // diff + color_self
```

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
ll lazy[1000005];

struct Fenwick {
    vector<ll> f;
    int n;
	using ll = long long;
	
    void init(int len) {
        n = len;
        f.resize(n + 1);
        memset(f.data(), 0, sizeof(ll) * (n + 1));
    }

    void add(int x, ll v) {
        for	(; x; x -= x & -x) f[x] += v;
    }

    void add(int x, int y, ll v) {
        add(y, v);
        if (x > 1) add( x - 1, -v);
    }

    ll query(int x) {
        ll res = 0;
		if (x <= 0 || x > n) return 0;
        for	(; x <= n; x += x & -x) res += f[x];
        return res;
    }
}bit;



void split(map<int, int>& node2color, int pos) {
	auto iter = node2color.lower_bound(pos);
	if (iter->first != pos) {
		node2color[pos] = prev(iter)->second;
	}
}

void assign(map<int, int>& node2color, int l, int r, int color) {
	auto it_l = node2color.find(l);
	auto it_r = node2color.find(r);
	for	(auto i = it_l; i != it_r; i++) {
        bit.add(i->first, next(i)->first - 1, lazy[i->second] - lazy[color]);	// diff = original - cur
    }
	node2color.erase(it_l, it_r);
    node2color[l] = color;
}

void solve() {
	cin >> n >> m;
	map<int, int> node2color;
	node2color[1] = 1; node2color[n + 1] = 0;
	bit.init(n);
	string q;
	rep (i, 0, m) {
		cin >> q;
		if (q[0] == 'C') {
			int l, r, color; cin >> l >> r >> color;
			split(node2color, l);
			split(node2color, r + 1);
			assign(node2color, l, r + 1, color);
		}
		if (q[0] == 'A') {
			int color, v; cin >> color >> v;
			lazy[color] += v;
		}
		if (q[0] == 'Q') {
			int i; cin >> i;
			auto iter = node2color.lower_bound(i);
			if (iter->first > i) {
				iter--;
			}
			cout << lazy[iter->second] + bit.query(i) << endl;
		}
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