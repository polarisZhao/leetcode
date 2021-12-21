



### 1. 遍历 BFS  & DFS

能够用 BFS 解决的问题，一定不要用 DFS 去做。 因为 DFS 的非递归写法很难写， 写出来面试官也看不懂， 而递归写法有可能会栈溢出。所以建议尽量使用 BFS。  BFS的非递归实现一般借助于队列。 

<img src="./z_img/dfs_bfs.jpeg" style="zoom:75%;" />

BFS 的模板：

~~~cpp
void dfs(vector<vector<char>>& grid, int i, int j, int m, int n){
    queue<pair<int, int> > m_que;
  
    m_que.push({i, j});
    while(!m_que.empty()){
        auto node = m_que.front();
        int r = node.first;
        int c = node.second;
        m_que.pop();

        if(r < 0 || c < 0 || r >= m || c >= n || grid[r][c] == '0')
            continue;

        grid[r][c] = '0';
        m_que.push({r-1, c});
        m_que.push({r+1, c});
        m_que.push({r, c-1});
        m_que.push({r, c+1});
    }
}
~~~



##### [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

~~~cpp
// dfs 解法
void dfs(vector<vector<char>>& grid, int i, int j, int m, int n){
    if(i >= m || j >= n || i < 0 || j < 0 || grid[i][j] == '0') return;
    grid[i][j] = '0';
    dfs(grid, i-1, j, m, n);
    dfs(grid, i+1, j, m, n);
    dfs(grid, i, j-1, m, n);
    dfs(grid, i, j+1, m, n);
}


int numIslands(vector<vector<char>>& grid) {
    int res = 0;
    if(grid.size()==0 || grid[0].size() == 0) return res;

    int m = grid.size();
    int n = grid[0].size();

    for(int i = 0; i < m; i++){
        for(int j = 0; j < n; j++){
            if(grid[i][j] == '1'){
                dfs(grid, i, j, m, n);
                res += 1;
            }
        }
    }
    return res;
}
~~~



~~~cpp
// bfs
void bfs(vector<vector<char>>& grid, int i, int j, int m , int n){
    queue<pair<int, int> > m_que;
    m_que.push({i, j});
    while(!m_que.empty()){
        auto node = m_que.front();
        int r = node.first;
        int c = node.second;
        grid[r][c] = '0';
        m_que.pop();

        if(r-1 >= 0 && grid[r-1][c] == '1') {
            m_que.push({r-1, c});
            // 直接 BFS，递归/队列 会出现相互包含的情况，最终导致爆了。每次元素入队之后，进行置零操作可以通过。
            grid[r-1][c] = '0';  
        }
      
        if(r+1 < m && grid[r+1][c] == '1') {
            m_que.push({r+1, c});
            grid[r+1][c] = '0';
        }
      
        if(c-1 >= 0 && grid[r][c-1] == '1'){
            m_que.push({r, c-1});
            grid[r][c-1] = '0';
        }
      
        if(c+1 < n && grid[r][c+1] == '1'){
            m_que.push({r, c+1});
            grid[r][c+1] = '0';
        }

    }
}


void bfs(vector<vector<char>>& grid, int i, int j, int m , int n){
    queue<pair<int, int> > m_que;
    m_que.push({i, j});
    while(!m_que.empty()){
        auto node = m_que.front();
        int r = node.first;
        int c = node.second;
        m_que.pop();
       // 退出判定
       if(r >= m || c >= n || r < 0 || c < 0 || grid[r][c] == '0')
            continue;

        grid[r][c] = '0';
        m_que.push({r-1, c});
        m_que.push({r+1, c});
        m_que.push({r, c-1});
        m_que.push({r, c+1});
    }
}


int numIslands(vector<vector<char>>& grid) {
    int res = 0;
    if(grid.size()==0 || grid[0].size() == 0) return res;

    int m = grid.size();
    int n = grid[0].size();


    for(int i = 0; i < m; i++){
        for(int j = 0; j < n; j++){
            if(grid[i][j] == '1'){
                bfs(grid, i, j, m, n);
                res += 1;
            }
        }
    }
    return res;
}
~~~

##### [695. 岛屿的最大面积](https://leetcode-cn.com/problems/max-area-of-island/)

~~~cpp
// dfs
void dfs(vector<vector<int>>& grid, int i, int j, int m, int n, int& area){
    if(i < 0 || j < 0 || i >= m || j >= n || grid[i][j] == 0) return;

    area++;
    grid[i][j] = 0;

    dfs(grid, i-1, j, m, n, area);
    dfs(grid, i+1, j, m, n, area);
    dfs(grid, i, j-1, m, n, area);
    dfs(grid, i, j+1, m, n, area);
}


int maxAreaOfIsland(vector<vector<int>>& grid) {
    if(grid.size() == 0 || grid[0].size() == 0 ) return 0;

    int max_area = 0;
    int m = grid.size();
    int n = grid[0].size();
    for(int i = 0; i < m; i++){
        for(int j = 0; j < n; j++){
            if(grid[i][j] == 1){
                int area = 0;
                dfs(grid, i, j, m, n, area);
                max_area = max(max_area, area);
            }
        }
    }
    return max_area;
}

// bfs
int bfs(vector<vector<int>>& grid, int i, int j, int m, int n){
    queue<pair<int, int> > m_que;

    int cnt = 0;
    m_que.push({i, j});

    while(!m_que.empty()){
        auto locate = m_que.front();
        int r = locate.first;
        int c = locate.second;
        m_que.pop();

        if(r < 0 || c < 0 || r >= m || c >= n || grid[r][c] == 0)
            continue;

        grid[r][c] = 0;
        cnt += 1;

        m_que.push({r-1, c});
        m_que.push({r+1, c});
        m_que.push({r, c-1});
        m_que.push({r, c+1});            
    }
    return cnt;
}


int maxAreaOfIsland(vector<vector<int>>& grid) {
    if(grid.size() == 0 || grid[0].size() == 0 ) return 0;

    int max_area = 0;
    int m = grid.size();
    int n = grid[0].size();
    for(int i = 0; i < m; i++){
        for(int j = 0; j < n; j++){
            if(grid[i][j] == 1){
                int area = bfs(grid, i, j, m, n);
                max_area = max(max_area, area);
            }
        }
    }
    return max_area;
}
~~~

##### [994. 腐烂的橘子](https://leetcode-cn.com/problems/rotting-oranges/)

~~~cpp
// 多源广度优先搜索
int orangesRotting(vector<vector<int>>& grid) {
    int times = 0;
    if(grid.size() == 0 || grid[0].size() == 0) return 0;

    int m = grid.size();
    int n = grid[0].size();

    int fresh = 0;
    queue<pair<int, int> > m_que;
    for(int i = 0; i < m; i++){
        for(int j = 0; j < n; j++){
            if(grid[i][j] == 2)  m_que.push({i, j});
            else if(grid[i][j] == 1)   fresh++;
        }
    }

    vector<pair<int, int> > directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

    while(!m_que.empty()){
        bool rotten = false;
        int len = m_que.size();
        for(int i = 0; i < len; i++){
            auto loc = m_que.front();
            m_que.pop();
            for(auto dir: directions){
                int x = loc.first + dir.first;
                int y = loc.second + dir.second;

                if(x < 0 || y < 0 || x >= m || y >= n || grid[x][y] != 1)
                    continue;

                grid[x][y] = 2;
                m_que.push({x, y});
                fresh--;
                rotten = true;
            }
        }
        if(rotten)  times++;
    }
    return fresh ? -1 : times;
}
~~~

##### [785. 判断二分图](https://leetcode-cn.com/problems/is-graph-bipartite/)

依照题意，就是将一个图的节点集合分割成两个独立的子集 A 和 B ，并使图中的每一条边的两个节点一个来自 A 集合，一个来自 B 集合 。最后看是否成立。

~~~cpp
// 将原问题转化为染色问题
bool isBipartite(vector<vector<int>>& graph) {
    int n = graph.size();
    if(n <= 1) return true;
    vector<int> color(n, 0); // 均未被染色

    // 遍历所有节点, 解决非连通图的问题
    for(int i = 0; i < n; i++){
        if(color[i] != 0) continue; 

        queue<int> Q;
        Q.push(i);
        color[i] = -1;  // -1 表示蓝色, 1 表示黄色
        while(!Q.empty()){
            auto idx = Q.front();
            Q.pop();

            for(auto neighbor: graph[idx]){
                if(color[neighbor] == color[idx]) return false; // 无法被正确染色
                // 对未染色节点进行染色
                if(color[neighbor] == 0){
                    color[neighbor] = -1 * color[idx];
                    Q.push(neighbor);
                }
            }
        }

    }
    return true;
}
~~~

##### [886. 可能的二分法](https://leetcode-cn.com/problems/possible-bipartition/)

~~~cpp
// 和题目 785 一样, 只是多添加了一个构图的过程
bool possibleBipartition(int n, vector<vector<int>>& dislikes) {
    vector<vector<int> > graph(n);
    for(int i = 0; i < dislikes.size(); i++){
        auto dislike = dislikes[i];
        graph[dislike[0]-1].push_back(dislike[1]-1);
        graph[dislike[1]-1].push_back(dislike[0]-1);
    }

    vector<int> color(n, 0);
    for(int i = 0; i < graph.size(); i++){
            if(color[i] != 0) continue;

            queue<int> m_que;
            m_que.push(i);
            color[i] = -1;

            while(!m_que.empty()){
                auto idx = m_que.front();
                m_que.pop();

                for(auto neighbor: graph[idx]){
                    if(color[neighbor] == color[idx]) return false;
                    else if(color[neighbor] == 0){
                        color[neighbor] = -1 * color[idx];
                        m_que.push(neighbor);
                    }
                }
            }
    }
    return true;
}

// TODO: 有时间使用 DFS 和并查集的方法写一遍
~~~

##### [133. 克隆图](https://leetcode-cn.com/problems/clone-graph/)

~~~cpp
Node* cloneGraph(Node* node) {
    if(node == nullptr) return nullptr;
    unordered_map<Node *, Node*> mp;

    queue<Node *> Q;
    Q.push(node);
    mp[node] = new Node(node->val);

    while(!Q.empty()){
        auto temp = Q.front();
        Q.pop();

        for(auto e: temp -> neighbors){
            if(mp.find(e) == mp.end()){
                Q.push(e);
                mp[e] = new Node(e->val);
            }
            mp[temp]->neighbors.push_back(mp[e]);
        }
    }
    return mp[node];
}
~~~



##### [797. 所有可能的路径](https://leetcode-cn.com/problems/all-paths-from-source-to-target/)

~~~cpp
// 涉及回溯的遍历题目, 最好使用 DFS 相关的写法
public:
    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
        if(graph.size() == 0) return res;

        stk.push_back(0);
        dfs(graph, 0, graph.size() - 1);
        return res;
    }

    void dfs(vector<vector<int>>& graph, int x, int n){
        if(x == n){
            res.push_back(stk);
            return;
        }
        
        for(auto y: graph[x]){
            stk.push_back(y);
            dfs(graph, y, n);
            stk.pop_back();
        }
    }
private:
    vector<vector<int> > res;
    vector<int> stk;
~~~

##### [261. 以图判树](https://leetcode-cn.com/problems/graph-valid-tree)

Ps: 该题暂不可见

满足两个条件：

- 刚好 N-1 条边 

- N个点连通

  

### 2. 拓扑排序

传说中，几乎每个公司都有一道拓扑排序的面试题  ！！！

拓扑排序一般用 BFS 来实现， 虽然也可以用 DFS 实现，但是如果用递归方式的 DFS 很可能会爆栈， 用非递归的方式需要借助于 stack， 实现起来比较复杂。

##### [207. 课程表](https://leetcode-cn.com/problems/course-schedule/)

~~~cpp
// 非常经典的拓扑排序题目
bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
    vector<int >  in_degree(numCourses, 0);
    vector<vector<int> > dict(numCourses, vector<int>());

    for(auto prerequisite: prerequisites){
        in_degree[prerequisite[0]]++;
        dict[prerequisite[1]].push_back(prerequisite[0]);
    }

    queue<int> Q; 
    for(int i = 0; i < in_degree.size(); i++){
        if(in_degree[i] == 0) Q.push(i); 
    }

    int cnt = 0;
    while(!Q.empty()){
        cnt++;
        auto x = Q.front(); Q.pop();
        auto courses = dict[x];
        for(auto course : courses){
            in_degree[course]--;
            if(in_degree[course] == 0)
                Q.push(course);
        }
    }

    return cnt == numCourses;
}
~~~

##### [210. 课程表 II](https://leetcode-cn.com/problems/course-schedule-ii/)

~~~cpp
vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
    vector<int >  in_degree(numCourses, 0);
    vector<vector<int> > dict(numCourses, vector<int>());

    for(auto prerequisite: prerequisites){
        in_degree[prerequisite[0]]++;
        dict[prerequisite[1]].push_back(prerequisite[0]);
    }

    queue<int> Q; 
    for(int i = 0; i < in_degree.size(); i++){
        if(in_degree[i] == 0) Q.push(i); 
    }

    int cnt = 0;
    vector<int > order;
    while(!Q.empty()){
        cnt++;
        auto x = Q.front(); Q.pop();
        order.push_back(x);
        auto courses = dict[x];
        for(auto course : courses){
            in_degree[course]--;
            if(in_degree[course] == 0)
                Q.push(course);
        }
    }

    return cnt == numCourses ? order : vector<int>();
}
~~~

##### [剑指 Offer II 115. 重建序列](https://leetcode-cn.com/problems/ur2n8P/)

判断是否只存在一个拓扑排序的序列，只需要保证队列中一直最多只有 1 个元素即可

~~~cpp
// 主站: https://leetcode-cn.com/problems/sequence-reconstruction/
bool sequenceReconstruction(vector<int>& org, vector<vector<int>>& seqs) {
    int n = org.size();
    vector<int> inorder(n+1);  // 入度
    vector<vector<int> > edges(n+1); // 边集
    
    // (1) 确保 org 中出现的节点 均在 seqs 中出现过
    unordered_set<int> nodes;
    int cnt = 0;
    for(auto seq: seqs){
        for(int i = 0; i < seq.size(); i++){
            nodes.insert(seq[i]);
            // (2) 不在 org 范围内
            if(seq[i] < 0 || seq[i] > n) return false;

            // 更新 inorder 和 edges
            if(i == 0) continue;
            edges[seq[i-1]].push_back(seq[i]);
            inorder[seq[i]]++;
        }
    }

    if(nodes.size() != n) return false;

    // 拓扑排序
    queue<int> Q;
    for(int i = 1; i <= n; i++){
            if(inorder[i] == 0) Q.push(i);  
    }
    int idx = 0;
    while(!Q.empty()){
        // (3) 队列有2个以上节点, 就有两种以上可能, 序列不唯一
        if(Q.size() > 1) return false;
        auto node = Q.front();  Q.pop();
        // (4) 匹配预设序列
        if(org[idx] != node) return false;
        idx++;

        for(int i = 1; i <= edges[node].size(); i++){
            if(--inorder[edges[node][i-1]] == 0){
                Q.push(edges[node][i-1]);
            }
        }
    }
    return idx == n;
}
~~~





### 3. UnionFind

##### [剑指 Offer II 116. 省份数量](https://leetcode-cn.com/problems/bLyHh0/)

~~~cpp
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
    for(int i = 0; i < fa.size(); i++)
        if(fa[i] == i) res++;

    return res;
}
~~~

##### [684. 冗余连接](https://leetcode-cn.com/problems/redundant-connection/)

~~~cpp
public:
    int find(int x){
        return fa[x] == x ? x : fa[x] = find(fa[x]);
    }

    void _union(int x1, int x2){
        int r1 = find(x1);
        int r2 = find(x2);
        if(r1 == r2) return;
        fa[r1] = r2;
    }

    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        fa.resize(n+1);

        for(int i = 0; i < fa.size(); i++)
            fa[i] = i;

        for(auto e: edges){
            if(find(e[0]) != find(e[1])){
                _union(e[0], e[1]);
            }else{
                return e;
            }
        }
        return vector<int>();
    }
private:
    vector<int> fa;
~~~

##### [323. 无向图中的连通分量数目](https://leetcode-cn.com/problems/number-of-connected-components-in-an-undirected-graph/)

Ps: 该题暂不可见



##### [130. 被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)

~~~cpp
private:
    vector<int> fa;
    int m;
    int n;

public:
    int find(int x){
        return x == fa[x] ? x : fa[x] = find(fa[x]);
    }

    void _union(int x1, int x2){
        int r1 = find(x1);
        int r2 = find(x2);

        if(r1 != r2) fa[r1] = r2;
    }

    bool is_connected(int x1, int x2){
        return find(x1) == find(x2);
    }

    int index(int i, int j){
        return i * n + j;
    }

    void solve(vector<vector<char>>& board) {
        if(board.size() == 0 || board[0].size() == 0) return;

        m = board.size();
        n = board[0].size();
        int dummy = m * n;

        fa.resize(dummy+1);
        for(int i = 0; i < fa.size(); i++) fa[i] = i;

        vector<pair<int, int> > directions = {{0, -1}, {0, 1}, {-1, 0}, {1, 0}};

        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(board[i][j] == 'O'){
                    if(i == 0 || i == (m-1) || j == 0 || j == (n-1)){  // 边界节点
                        _union(index(i, j), dummy);
                    }else{  // 非边界节点
                        for(auto dir: directions){
                            int x = i + dir.first;
                            int y = j + dir.second;
                            if(board[x][y] == 'O')
                                _union(index(i, j), index(x, y));
                        }
                    }
                }
            }
        }

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 'O') {
                    if (!is_connected(index(i, j), dummy)) {
                        board[i][j] = 'X';
                    }
                }
            }
        }

    }
~~~

### 4. 最小生成树

##### [1135 最低成本连通所有城市](https://leetcode-cn.com/problems/connecting-cities-with-minimum-cost/)

Ps: 该题暂不可见



### 5. 最短路径

##### [743. 网络延迟时间](https://leetcode-cn.com/problems/network-delay-time/)

Dijkstra 与 floyd 的区别? 

`Dijkstra` 不能处理负权图，且主要处理单源最短路；`Floyd` 能处理负权图，主要处理多源最短路问题。

其基本思想也不同： `Dijkstra` 基于贪心思想，`Floyd` 则是基于动态规划思想。

~~~cpp
// Dijkstra
int networkDelayTime(vector<vector<int>>& times, int n, int k) {
    // 建图
    vector<vector<int>> g(n + 1, vector<int>(n + 1, INT_MAX / 2));
    for (auto& it : times) {
        g[it[0]][it[1]] = it[2]; 
    }
    vector<int> dis(n + 1, INT_MAX / 2), visit(n + 1, 0); // dis[i] 代表源点到点 i 的最短路，
                                                          // 初始化为 INT_MAX / 2 防止溢出

    dis[k] = 0;
    for (int cnt = 0; cnt < n; cnt++) {  // 查找 n 次: 包含源点
        // 找到离源点最短的点，并记录
        int book = 0;
        for (int i = 1; i <= n; i++) {
            if (dis[i] < dis[book] && !visit[i]) {
                book = i;
            }
        }
        if (book == 0) return -1; // 如果源点无法到达任何一个点,直接返回

        visit[book] = 1;
        for (int i = 1; i <= n; i++) {   // 松弛操作, 以 book 为中心点进行扩展 
            if (g[book][i] != INT_MAX / 2) {
                dis[i] = min(dis[i], dis[book] + g[book][i]);
            }               
        }
    }

    return  *max_element(dis.begin()+1, dis.end());
}

// Floyd:
int networkDelayTime(vector<vector<int>>& times, int n, int k) {
    // 建图
    vector<vector<int>> dp(n + 1, vector<int>(n + 1, INT_MAX / 2));
    for (auto& it : times)  dp[it[0]][it[1]] = it[2];
    for (int i = 1; i <= n; i++)    dp[i][i] = 0;

    // floyd
    for (int k = 1; k <= n; k++) {
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                dp[i][j] = min(dp[i][j], dp[i][k] + dp[k][j] );
            }
        }
    }
    int ret = 0;
    for (int i = 1; i <= n; i++)    ret = max(ret, dp[k][i]);
    return ret == INT_MAX / 2 ? -1 : ret;
}
~~~

