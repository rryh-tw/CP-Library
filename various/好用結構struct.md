區間合併（總和、連續最大最小、前綴最大最小、後綴最大最小）
```cpp
by abc864197532 https://codeforces.com/contest/1843/submission/210427134
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
