

### 解题技巧

- 基本模板：

  ##### DFS

  ~~~cpp
  // Template 1: Traverse
  void traverse(TreeNode root) {
      if (root == null)  return;
      
      // do something with root
      traverse(root->left);
      // do something with root
      traverse(root->right);
      // do something with root
  }
  
  
  // Tempate 2: Divide & Conquer
  ResultType traversal(TreeNode root) {
      // null or leaf
      if (root == null) {
          // do something and return;
      }
  
      // Divide
      ResultType left = traversal(root->left);
      ResultType right = traversal(root->right);
  
      // Conquer
      ResultType result = Merge from left and right.
      return result;
  }
  ~~~







扩展： 如何求不同形状 ?  如何保存路径





扩展： 如何求最大周长



课程表两道







目标和





字符串 解码





矩阵中的最长递增路径



### 3. 并查集

并查集(union & find) 是一种树型的数据结构，用于处理一些不交集的合并以及查询问题。

**Find**： 确定元素属于哪一个子集。 它可以用来确定两个元素是否属于同一个子集。

**Union**: 将两个子集合并成同一个集合。

##### [547. 省份数量](https://leetcode-cn.com/problems/number-of-provinces/)

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
    for(int i = 0; i < fa.size(); i++){
        if(fa[i] == i) res++;
    }

    return res;
}
~~~

##### [128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

~~~cpp
// WXG 一面、神策数据
public:
int find(int x){
    return x == parent[x] ? x : parent[x] = find(parent[x]);
}

int _union(int x, int y){
    if(parent.find(y) == parent.end()) return 1;

    int root1 = find(x);
    int root2 = find(y);
    if(root1 != root2){
        parent[root2] = root1;
        cnt[root1] += cnt[root2];
    } 
    return cnt[root1];
}

int longestConsecutive(vector<int>& nums) {
    int res = 0;
    if(nums.size() == 0) return res;
    // init
    for(auto num: nums){
        parent[num] = num;
        cnt[num] = 1;
    }

    // union
    for(int i = 0; i < nums.size(); i++){
        res = max(res, _union(nums[i], nums[i]+1));
    }
    return res;
}
private:
    unordered_map<int, int> parent;
    unordered_map<int, int> cnt;
~~~

##### [130. 被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)

参考链接： https://leetcode-cn.com/problems/surrounded-regions/solution/zhua-zhong-dian-san-chong-jie-fa-jian-ji-d621/
