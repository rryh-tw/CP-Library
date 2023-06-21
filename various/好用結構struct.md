### 區間合併（總和、連續最大最小、前綴最大最小、後綴最大最小）
```cpp
// by abc864197532 https://codeforces.com/contest/1843/submission/210427134
struct state {
    int premn, sufmn, premx, sufmx, mn, mx, sum;
    state (int x = 0) {
        premn = sufmn = mn = min(x, 0);
        premx = sufmx = mx = max(x, 0);
        sum = x;
    }
    state operator + (const state &o) {
        state ans;
        ans.premn = min(premn, sum + o.premn);
        ans.sufmn = min(o.sufmn, sufmn + o.sum);
        ans.premx = max(premx, sum + o.premx);
        ans.sufmx = max(o.sufmx, sufmx + o.sum);
        ans.mn = min({mn, o.mn, sufmn + o.premn});
        ans.mx = max({mx, o.mx, sufmx + o.premx});
        ans.sum = sum + o.sum;
        return ans;
    }
    state rev() {
        state ans = *this;
        swap(ans.premn, ans.sufmn), swap(ans.premx, ans.sufmx);
        return ans;
    }
};
```
### zebra序列合併
`{1,−1,1,−1,1}`
`rsum`: 後綴長度
`rv`: right value
```cpp
// by rryh
struct Node {
    int lv, rv;
    int ans, lsum, rsum;
    bool full;
    Node(int val):lv(val), rv(val), ans(0), full(1), rsum(1), lsum(1){}
    Node():lv(0), rv(0), ans(0), full(0), rsum(0), lsum(0){}
    friend Node operator+(const Node &L, const Node &R) {
        Node t(0);
        bool flag = (L.rv * R.lv == -1);
        t.lv = L.lv;
        t.rv = R.rv;
        t.lsum = L.lsum + (flag && L.full)*R.lsum; 
        t.rsum = R.rsum + (flag && R.full)*L.rsum;
        t.full = (flag && L.full && R.full);
        t.ans = L.ans + R.ans + (flag ? (L.rsum + R.lsum >> 1) - (L.rsum >> 1) - (R.lsum >> 1) : 0);
        return t;
    }
} t[maxn*4];
```
