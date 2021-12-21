

##### 并查集

大部分此类题目都可以使用DFS、BFS 和 并查集三种方法来进行解题。 这里主要说明并查集。

并查集提供两个操作: `find` 和 `union`。 其中前者 `find` 用于获取根节点， 后者 `__union` 则用来进行进行合并操作。

并查集可以用来计算图中连通分量的个数和环的数量。

~~~cpp
// 华为二面

int find(vector<int> & fa, int i){
    return i == fa[i] ? i : fa[i] = find(fa, fa[i]);
}

void _union(vector<int>& fa, int i, int j){
    fa[find(fa, i)] = find(fa, j);
}

int findCircleNum(vector<vector<int>>& isConnected) {
    int n = isConnected.size();
    vector<int> fa(n, 0);

    for(int i = 0; i < fa.size(); i++){
        fa[i] = i;
    }

    for(int i = 0; i < n; i++){
        for(int j = i+1; j < n; j++){
            if(isConnected[i][j]) _union(fa, i, j);
        }
    }

    int res = 0;
    for(int i = 0; i < fa.size(); i++){
        if(fa[i] == i) res++;
    }

    return res;
}
~~~

参考链接： https://algs4.cs.princeton.edu/15uf/

