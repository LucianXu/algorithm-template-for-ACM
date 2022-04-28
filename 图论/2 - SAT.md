# 2 - SAT

```c++
int n, m;
int e[N], ne[N], h[N], idx;
int dfn[N], low[N], stk[N], belong[N], timestamp, top, scc_cnt;
bool in_stk[N];
void add(int a, int b){
    e[idx] = b;
    ne[idx] = h[a];
    h[a] = idx++;
}
void tarjan(int u){
    ···
}
bool check(){
    for(int i = 1; i < 2 * n; i += 2){
        if(belong[i] == belong[i + 1]) return 0;
    }
    return 1;
}
int opp(int i){
	···
}
int main(){
    // 添边 //
    // add(a, opp(b)); //
    // add(b, opp(a)); //
    // tarjan //
    if(check())···
    else ···
    return 0;
}
```

上述将 i 与 i + 1 作为一个集合里的元素，还有将 i 与 i + n 作为一个集合里元素的做法。

若要输出选取方式。

```c++
if(check()){
    for(int i = 1; i < 2 * n; i += 2){
        if(belong[i] > belong[i + 1]) printf("%d\n", i + 1);
        else printf("%d\n", i);
    }
}
```

取 belong[] 更小的。

