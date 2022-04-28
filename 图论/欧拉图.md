# 欧拉图

## Hierholzer算法

### 有向图

```c++
struct node{
    int to;
    bool exist;
};
vector<node> edge[N];
vector<int> ans;
int n, m, flag1, flag2;
int d[N], last[N];
bool cmp(node a, node b){
    return a.to < b.to;
}
void hierholzer(int u){
    for(int i = 0; i < edge[u].size(); i = max(i, last[u]) + 1){
        // 比i++能加速 //
        if(edge[u][i].exist){
            edge[u][i].exist = 0;
            last[u] = i;
            hierholzer(edge[u][i].to);
        }
    }
    ans.push_back(u);
}
bool check(){
    for(int i = 1; i <= n; i++){
        if(d[i] > 1 || d[i] < -1) return 0;
        if(d[i] == 1) flag1++;
        else if(d[i] == -1) flag2++;
    }
    if(flag1 > 1 || flag2 > 1) return 0;
    return 1;
}
int main(){
    /* 边：a -> b
        scanf("%d%d", &a, &b);
        edge[a].push_back((node){b, 1});
        d[a]++;
        d[b]--;
    */
    for(int i = 1; i <= n; i++){
        sort(edge[i].begin(), edge[i].end(), cmp);
    }
    // 要求字典序最下，对边排序 //
    if(!check()){
        cout << "No" << endl;
        return 0;
    }
    int id = 1;
    for(int i = 1; i <= n; i++){
        if(d[i] == 1){
            id = i;
            break;
        }
    }
    hierholzer(id);
    for(int i = ans.size() - 1; i >= 0; i--){
        printf("%d ", ans[i]);
    }
    return 0;
}
```

### 无向图

```c++
struct node{
    int to, revref;
    bool exist;
};
vector<node> edge[N];
vector<int> ans;
int n, m, flag;
int d[N], reftop[N], last[N];
bool cmp(node a, node b){
    return a.to < b.to;
}
void hierholzer(int u){
    for (int i = 0; i < edge[u].size(); i = max(i, last[u]) + 1){
        // 比i++能加速 //
        if(edge[u][i].exist){
            auto t = edge[u][i];
            t.exist = 0;
            edge[t.to][t.revref].exist = 0;
            last[u] = i;
            hierholzer(t.to);
        }
    }
    ans.push_back(u);
}
bool check(){
    for(int i = 1; i <= n; i++){
        if(d[i] % 2 == 1) flag++;
    }
    if(flag == 0 || flag == 2) return 1;
    return 0;
}
int main(){
    /* 边：a -> b
        scanf("%d%d", &a, &b);
        edge[a].push_back((node){b, 0, 1});
        edge[b].push_back((node){a, 0, 1});
        d[a]++;
        d[b]++;
    */
    for(int i = 1; i <= n; i++){
        sort(edge[i].begin(), edge[i].end(), cmp);
    }
    for(int i = 1; i <= n; i++){
        for(int j = 0; j < edge[i].size(); j++){
            edge[i][j].revref = reftop[edge[i][j].to]++;
        }
    }
    if(!check()){
        cout << "No" << endl;
        return 0;
    }
    int id = 0;
    for(int i = 1; i <= n; i++){
        if(!d[id] && d[i]) id = i;
        else if(!(d[id] & 1) && (d[i] & 1)) id = i;
    }
    hierholzer(id);
    for(int i = ans.size() - 1; i >= 0; i--){
        cout << ans[i] << endl;
    }
    return 0;
}
```

