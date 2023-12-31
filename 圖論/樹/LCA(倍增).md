```cpp
// by rryh
struct LCA {
    vector<int> dep;
    vector<array<int, 35> > p;
    int n;
    LCA(vector<vector<int> > &G, int root = 0) {
        n = G.size();
        p.resize(n);
        dep.resize(n);

        auto dfs = [&](auto self, int v, int par) -> void {
            p[v][0] = par;
            dep[v] = dep[par] + 1;
            for(int i = 1; i <= __lg(dep[v]); i++) {
                p[v][i] = p[p[v][i-1]][i-1];
            }

            for (int chd : G[v]) if(chd != par) {
                self(self, chd, v);
            }
        };
        dfs(dfs, root, root);
    }


    int lca(int x, int y) {
        if(dep[x] > dep[y]) swap(x, y);
        //y jump
        for(int d = dep[y] - dep[x]; d; d -= d&(-d)) {
            int jp = __builtin_ctzll(d);
            y = p[y][jp];
        }

        for(int i = __lg(n); i >= 0; i--) {
            if(p[x][i] != p[y][i]) {
                x = p[x][i]; y = p[y][i];
            }
        }

        return (x == y ? x : p[x][0]);
    }
    
};
```
### 預處理資訊
```cpp
// 寫在dfs處理倍增迴圈裡
ma[pos][i] = max(ma[pos][i-1], ma[p[pos][i-1]][i-1]); // 最大值
sum[v][i] = sum[v][i-1] + sum[p[v][i-1]][i-1]; // 總和
```
### 取得x, y路徑上的資訊
```cpp
int query(int x, int y) {
    int l = lca(x, y);
    int left = 0, right = 0;
    for(int d = dep[x] - dep[l]; d; d -= d&(-d)) {
        int jp = __builtin_ctzll(d);
        left = left + sum[x][jp];
        x = p[x][jp];
    }
    for(int d = dep[y] - dep[l]; d; d -= d&(-d)) {
        int jp = __builtin_ctzll(d);
        right = right + sum[y][jp];
        y = p[y][jp];
    }
    return left + right;
}
 ```
