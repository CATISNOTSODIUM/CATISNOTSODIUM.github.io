# Minimizing the sum of the minimum and maximum edge weights along a path.
> Source: [Codeforce 2117G - Omg Graph](https://codeforces.com/problemset/problem/2117/G)

Our goal is to find the path that minimize the cost, which is defined as the sum of the minimum and maximum edge weights along a path from node $1$ to $n$, $C = \min_{i = 1}^{k} w_i + \max_{i = 1}^{k}w_i$.

The tricky part is that the path we are interested in is not necessarily simple (allowing repeated vertices and edges). You can sidetrack yourself by visiting minimum edge weight to lower the value of $\min_{i = 1}^{k} w_i$. For example, consider this graph.

``` mermaid
%%{init: {'theme': 'default', 'flowchart': { 'useMaxWidth': true, 'htmlLabels': true }}}%%
graph LR
    A((1)) ---|1| B((2))
    B ---|10| C((3))
    C ---|2| A
```
If the path is simple, it is trivial that the minimum cost path is $1 \rightarrow 3$ with the cost of $2 + 2 = 4$. However, since repeated vertices are allowed, you can walk from $1 \rightarrow 2 \rightarrow 1$ before $1 \rightarrow 3$. The total cost is $1 + 2 = 3$.

There are many solutions to this problem. However, the approach that I came up (sadly not during contest) is quite similar to Kruskal's algorithm.

## Intuition
For a graph $G$, we use a tree subgraph $T$ to represent the path of interest. Without simple path constraint, the minimum/maximum edge weights along the path is simply the minimum edge of $T$. For example, traversing $1 \to 2 \to 1 \to 4 \to 5 \to 4 \to 1 \to 6 \to 7 \to 8$ will give us this tree. The minimum edge weight is $4$.
``` mermaid
%%{init: {'theme': 'default', 'flowchart': { 
    'useMaxWidth': true, 'htmlLabels': true }}}%%
graph LR
    1((1)) ---|6| 2((2))
    2 ---|5| 3((3))
    3 ---|6| 8((8))
    1 ---|7| 4((4))
    4 ---|4| 5((5))
    5 ---|7| 8
    1 ---|5| 6((6))
    6 ---|5| 7((7))
    7 ---|5| 8
    linkStyle 6 stroke:#0f0,stroke-width:2px
    linkStyle 7 stroke:#0f0,stroke-width:2px
    linkStyle 8 stroke:#0f0,stroke-width:2px
    linkStyle 0 stroke:#0f0,stroke-width:2px
    linkStyle 3 stroke:#0f0,stroke-width:2px
    linkStyle 4 stroke:#0f0,stroke-width:2px

```

One naive (and impractical) way of solving this problem is by going through all possible tree subgraphs that contain node $1$ and $n$. This is impossible since the number of tree subgraphs grows exponentially with the number of nodes. (Spanning trees already take $n^{n - 2}$ possibilities.)

One simple trick that save us is constraining our graph maximum edge $w_{max}$. If $w_max$ is too small, node $1$ and node $n$ are disconnected. So we have to crank up $w_{max}$ until node $1$ and node $n$ are connected. Alternatively, we sort the edges based on weights from small to large. Then, start adding new edges until node $1$ and node $n$ are connected.  This approach is similar to Kruskal's algorithm and can be achieved by using Disjoint Set Union (DSU).

Throughout the process, we can augment the minimum edge and maximum edge weights of each component and update every time we perform union operation. 

If node $1$ and node $n$ are in the same component, we could calculate the cost (simply adding minimum and maximum edges augmented in the component) can call it a day. However, we should continue adding new edges, as some high-weight edges may unlock new minimum-weight edges during the process. So, we have to track the mininum cost and only halt when the weight of the new edge alone is larger than the minimum cost.   

The time complexity of this solution is $O(E \log{E})$ since we have to sort the edges. The time taken in DSU is $O(E \cdot \alpha{(n)})$ which is small compared to sorting time.

## Code
```cpp
#define vi vector<int>
#define M 1000000007
#define P 31
#define N 64
 
void solve() {
    int n, m; cin >> n >> m;
    map<int, map<int, int>> dist;
    vii e;
    for (int i = 0; i < m; i++) {
        int u, v, w; cin >> u >> v >> w; u--; v--;
        dist[u][v] = w; dist[v][u] = w;
        e.push_back({w, u, v});
    }
    sort(e.begin(), e.end());
    vi dsu(n, -1);  vi rank(n, 1); 
    vi minEdge(n, INT_MAX);
    vi maxEdge(n, -1);
    for (int i = 0; i < n; i++) dsu[i] = i;
    function<int(int)> find_dsu = [&](int u){
        if (u == dsu[u]) return u;
        dsu[u] = find_dsu(dsu[u]);
        return dsu[u];
    };
    function<void(int, int)> union_dsu = [&](int u, int v) {
        int a = find_dsu(u); 
        int b = find_dsu(v);
        if (a == b) return;
        if (rank[a] > rank[b]) {
            swap(a, b);
        }
        dsu[a] = b; 
        rank[b] += rank[a]; 
        int mn = min(minEdge[a], minEdge[b]); 
        mn = min(mn, dist[u][v]);
        int mx = max(maxEdge[a], maxEdge[b]);
        mx = max(mx, dist[u][v]);
        minEdge[a] = minEdge[b] = mn;
        maxEdge[a] = maxEdge[b] = mx;
        return;
    };
    int mx = -1; int ans = INT_MAX;
    for (int i = 0; i < e.size(); i++) {
        vi E = e[i];
        union_dsu(E[1], E[2]);
        if (find_dsu(0) == find_dsu(n - 1)) {
            mx = E[0]; if (mx > ans) break;
            ans = min(ans, mx + minEdge[find_dsu(0)]);
        }
    }
    cout << ans << "\n";
}

int main() {  
	int tc; cin >> tc;
    while (tc--) solve();
	return 0;
}
```