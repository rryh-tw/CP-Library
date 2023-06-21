```cpp
vector<array<int, 35> >  p(n);

vector <int> dep(n);
auto dfs = [&](int v, int par) -> void {
  
    for (int chd : adj[v]) if(chd != par) {
        jump[u][0] = v;
        dep[u] = dep[v] + 1;
        for (int i = 1; i < 35; ++i) {
            int k = jump[u][i - 1];
            if (~k) {
                jump[u][i] = jump[k][i - 1];
            }
        }
        dfs(u);
    }
    out[v] = _t++;
};
