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
