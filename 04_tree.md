## Leetcode Tree

#### (1) 常用技巧总结

- leetcode的测试集经常会有 [] , [0]，所以很多题目先要考虑判断是否为空，return None 或者 return [ ]。

- 时刻要考虑这个节点是否为空， 空节点是不能访问左子树和右子树的。

- 当返回值非 void 时，考虑到递归的返回值问题，大部分需要设置一个 helper 函数。但是不是绝对，可以利用这个返回值。

- 递归的基本思想

  ```
  def func(root):
      if 满足条件: 退出或进行相关操作
      
      // 相关操作1;
      func(root->left); // 递归左子树
      // 相关操作2;
      func(root->right);  //递归右子树
      // 相关操作3;
  ```

- 关于递归：
  (1) 递归相较于迭代：浪费资源反复调用函数

  - 递归是一个树结构，每个分支都探究到最远，发现无法继续的时候往回走，每个节点只会访问一次
  - 迭代是一个环结构，每次迭代都是一个圈，不会拉掉其中的某一步，然后不断循环，每个节点都会被循环访问

  (2) 思考点:

  - 关于递归：退出条件、递归任务、返回值
  - 关于子树：根节点、左子树、右子树
  - 关于特殊条件处理：节点 root 为空， 叶子节点`root->left && root->right` 为空、 单子树为空 `root->left || root->right` 、正常节点

  (3) 基本思路

  - 利用递归先把子问题求解出来(左子树和右子树)
  - 考虑如何利用子树的结果来获取当前的结果
  - 在前面添加上退出条件

借助与二叉树来理解 DFS & BFS 的思想





~~~cpp
Template 1: Traverse

public class Solution {
    public void traverse(TreeNode root) {
        if (root == null) {
            return;
        }
        // do something with root
        traverse(root.left);
        // do something with root
        traverse(root.right);
        // do something with root
    }
}


Tempate 2: Divide & Conquer

public class Solution {
    public ResultType traversal(TreeNode root) {
        // null or leaf
        if (root == null) {
            // do something and return;
        }
        
        // Divide
        ResultType left = traversal(root.left);
        ResultType right = traversal(root.right);
        
        // Conquer
        ResultType result = Merge from left and right.
        return result;
    }
}
~~~





~~~
public class Solution {
    public ArrayList<ArrayList<Integer>> levelOrder(TreeNode root) {
        ArrayList result = new ArrayList();
        
        if (root == null)
            return result;
            
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            ArrayList<Integer> level = new ArrayList<Integer>();
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode head = queue.poll();
                level.add(head.val);
                if (head.left != null)
                    queue.offer(head.left);
                if (head.right != null)
                    queue.offer(head.right);
            }
            result.add(level);
        }
        
        return result;
    }
}
~~~





#### （2）常见题目

##### 1. 基本遍历

- [遍历] 前序 (二叉树展开为链表)、中序(第k大的节点、是否是BST)、后序、层序遍历(常规、按层、之字型、右视图、二叉树的宽度、左叶子之和)   | 要求递归和迭代均会

前序：

中序：

后序：

层序：

##### 2. 分治思想的应用

(1) 根节点的这个问题， 和子节点(左子树 & 右子树)是什么关系

(2) 递归退出的条件

- [性质判定]  最大深度、最小深度、平衡树、相同的树、对称二叉树、翻转二叉树
- [公共祖先]  最近公共祖先(二叉树、二叉搜索树)
- 

##### 3、BFS 和 DFS 的应用

- [路径和]  二叉树的路径和、所有路径、最大路径
- [vector 和 tree 交互] 序列化和反序列化二叉树、有序链表重建二叉搜索树、重建二叉树



合并二叉树、二叉树节点的插入和删除







#### 题型 1 遍历 

前序遍历、中序遍历、后序遍历、层序遍历(普通、分层、锯齿)

##### [144. 二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

~~~cpp
// 滴滴一面 
vector<int> preorderTraversal(TreeNode* root) {
    stack<TreeNode *> m_stk;

    vector<int> res;
    TreeNode * p = root;
    while(p || !m_stk.empty()){
        if(p){
            res.push_back(p -> val);
            m_stk.push(p);
            p = p -> left;
        }else{
            p = m_stk.top();
            m_stk.pop();
            p = p -> right;
        }
    }
    return res;
}


vector<int> preorderTraversal(TreeNode* root) {
    vector<int> res;
    if(root == NULL) return res;

    stack<TreeNode *> m_stk;
    m_stk.push(root);
    while(!m_stk.empty()){
        TreeNode * cur = m_stk.top();
        m_stk.pop();
        res.push_back(cur->val);
        if(cur->right) m_stk.push(cur->right);
        if(cur->left) m_stk.push(cur->left);
    }
    return res;
}
~~~

##### [114. 二叉树展开为链表](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)

~~~cpp
// 使用第二种遍历方法比较好操作
void flatten(TreeNode* root) {
    if(root == NULL) return;

    stack<TreeNode *> m_stk;
    m_stk.push(root);
    TreeNode *prev = nullptr;
    while(!m_stk.empty()){
        TreeNode * cur = m_stk.top();
        if(prev != nullptr){
            prev -> left = NULL;
            prev -> right = cur;
        }

        m_stk.pop();
        if(cur->right) m_stk.push(cur->right);
        if(cur->left) m_stk.push(cur->left);

        prev = cur;
    }
}
~~~







##### [94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

~~~cpp
vector<int> inorderTraversal(TreeNode* root) {
    vector<int> res;
    if(root == NULL) return res;

    stack<TreeNode *> m_stk;
    TreeNode * ptr = root;
    while(ptr || !m_stk.empty()){
        if(ptr){
            m_stk.push(ptr);
            ptr = ptr -> left;
        }else{
            ptr = m_stk.top();
            res.push_back(ptr->val);
            m_stk.pop();
            ptr = ptr -> right;
        }
    }
    return res;
}
~~~



##### [145. 二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)

~~~cpp
vector<int> postorderTraversal(TreeNode* root) {
    vector<int> res;
    if(root == NULL) return res;
    stack<TreeNode *> m_stk;

    m_stk.push(root);
    while(!m_stk.empty()){
        TreeNode * ptr = m_stk.top();
        res.push_back(ptr->val);
        m_stk.pop();
        if(ptr->left) m_stk.push(ptr->left);
        if(ptr->right) m_stk.push(ptr->right);
    }

    reverse(res.begin(), res.end());
    return res;
}
~~~



##### [剑指 Offer 32 - I. 从上到下打印二叉树](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)

~~~cpp
vector<int> levelOrder(TreeNode* root) {
    vector<int>  res;
    if(root == NULL) return res;

    queue<TreeNode * > m_queue;
    m_queue.push(root);

    while(!m_queue.empty()){
        TreeNode * ptr = m_queue.front();
        res.push_back(ptr->val);
        if(ptr->left) m_queue.push(ptr->left);
        if(ptr->right) m_queue.push(ptr->right);
        m_queue.pop();
    }
    return res;
}
~~~



##### [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

~~~cpp
// 天眼查一面、字节后端二面(可否用递归实现)、得物一面、商业化后端一面、lazada广告、小米
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int> > res;
    if(root == NULL) return res;

    queue<TreeNode * > m_queue;
    m_queue.push(root);

    while(!m_queue.empty()){
        int len = m_queue.size();
        vector<int> rows;
        for(int i = 0; i < len; i++){
            rows.push_back(m_queue.front()->val);
            if(m_queue.front()->left) m_queue.push(m_queue.front()->left);
            if(m_queue.front()->right) m_queue.push(m_queue.front()->right);
            m_queue.pop();
        }
        res.push_back(rows);
    }
    return res;
}
~~~

##### [103. 二叉树的锯齿形层序遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

~~~cpp
vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
    vector<vector<int> > res;
    if(root == NULL) return res;
    deque<TreeNode *> m_deq; // 双向队列
    m_deq.push_back(root);

    bool flag = true;
    while(! m_deq.empty()){
        vector<int> rows;
        int n =  m_deq.size();
        if(flag){
            for(int i = 0; i < n; i++){
                TreeNode * ptr = m_deq.front();
                rows.push_back(ptr->val);
                if(ptr->left) m_deq.push_back(ptr->left);
                if(ptr->right) m_deq.push_back(ptr->right);
                m_deq.pop_front();
            }

        }else{
            for(int i = 0; i < n; i++){
                TreeNode * ptr = m_deq.back();
                rows.push_back(ptr->val);
                if(ptr->right) m_deq.push_front(ptr->right);
                if(ptr->left) m_deq.push_front(ptr->left);
                m_deq.pop_back();
            }
        }
        flag = !flag;
        res.push_back(rows);
    }
    return res;
}
~~~

##### [107. 二叉树的层序遍历 II](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)

~~~cpp
vector<vector<int>> levelOrderBottom(TreeNode* root) {
    vector<vector<int> > res;
    if(root == NULL) return res;

    queue<TreeNode * > m_queue;
    m_queue.push(root);

    while(!m_queue.empty()){
        int len = m_queue.size();
        vector<int> rows;
        for(int i = 0; i < len; i++){
            rows.push_back(m_queue.front()->val);
            if(m_queue.front()->left) m_queue.push(m_queue.front()->left);
            if(m_queue.front()->right) m_queue.push(m_queue.front()->right);
            m_queue.pop();
        }
        res.push_back(rows);
    }
    reverse(res.begin(), res.end());
    return res;
}
~~~

##### [958. 二叉树的完全性检验](https://leetcode-cn.com/problems/check-completeness-of-a-binary-tree/)

~~~cpp
// 对于一个完全二叉树, 层序遍历的过程中遇到第一个空节点之后不应该再出现非空节点
bool isCompleteTree(TreeNode* root) {
    queue<TreeNode *> m_queue;
    m_queue.push(root);
    bool reachedEnd = false;
    while(!m_queue.empty()){
        TreeNode * node = m_queue.front();
        m_queue.pop();
        if(node == NULL){
            reachedEnd = true;
            continue;
        }else{
            if(reachedEnd) return false;
            m_queue.push(node->left);
            m_queue.push(node->right);
        }
    }
    return true;
}
~~~

##### [662. 二叉树最大宽度](https://leetcode-cn.com/problems/maximum-width-of-binary-tree/)

~~~cpp
// 字节二面

// 利用完全二叉树的性质:
// 对于一颗完全二插树，如果按照从上至下，从左往右对所有节点从零开始顺序编号
// 则父节点的左孩子节点的序号：2i, 父节点的左孩子节点的序号：2i+1;
//
int widthOfBinaryTree(TreeNode* root) {
    int res = 0;
    if(root == NULL) return res;

    queue<pair<TreeNode*, int> > mq;
    mq.push({root, 1});

    while(!mq.empty()){
        res = max(res, mq.back().second - mq.front().second + 1);
        int n = mq.size();
        int t = mq.front().second - 1; // 主要是为了防止溢出, 保证节点相对位置不变

        for(int i = 0; i < n; i++){
            TreeNode * node = mq.front().first;
            int idx = mq.front().second - t;
            mq.pop();
            if(node->left) mq.push({node->left, 2 * idx});
            if(node->right) mq.push({node->right, 2 * idx + 1});
        }
    }
    return res;
}
~~~





#### 2. 性质判定

##### [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

~~~cpp
// 要求会迭代写法
int maxDepth(TreeNode* root) {
    if(root == NULL) return 0;
    return max(maxDepth(root->left), maxDepth(root->right)) + 1;
}

// N 叉树的最大深度 -> 腾讯文档一面
int maxDepth(Node* root) {
    if(root == NULL) return 0;

    int children_depth = 0;
    for(auto node: root->children){
        children_depth = max(children_depth, maxDepth(node));
    }

    return children_depth + 1; 
}
~~~

##### [101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

~~~cpp
bool symmetric_help(TreeNode * left, TreeNode * right){
    if(left == NULL && right == NULL) return true;
    if(left == NULL || right == NULL) return false;
    return  symmetric_help(left->left, right->right) 
            && symmetric_help(left->right, right->left) 
            && left -> val == right -> val;
}

bool isSymmetric(TreeNode* root) {
    if(root == NULL) return true;
    return symmetric_help(root->left, root-> right);
}
~~~

##### [100. 相同的树](https://leetcode-cn.com/problems/same-tree/)

~~~cpp
bool isSameTree(TreeNode* p, TreeNode* q) {
    if(p == NULL && q == NULL) return true;
    if(p == NULL || q == NULL) return false;
    return p -> val == q -> val && isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
}
~~~

##### [110. 平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)

~~~cpp
// 快手: 2020.09  后续遍历思路
int getMaxDepth(TreeNode * root){
    if(root == NULL) return 0;
    return 1 + max(getMaxDepth(root->left), getMaxDepth(root->right));
}
bool isBalanced(TreeNode* root) {
    if(root == NULL) return true;
    int left = getMaxDepth(root->left);
    int right = getMaxDepth(root->right);
    return abs(left - right) <= 1 && isBalanced(root->left) && isBalanced(root->right);
}
~~~

##### [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

~~~cpp
// 美团三面: 要求用迭代实现
TreeNode* invertTree(TreeNode* root) {
    if(root == NULL) return NULL;

    TreeNode * tmp = root->left;
    root->left = invertTree(root->right);
    root->right = invertTree(tmp);
    return root;
}
~~~

##### [111. 二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

~~~cpp
  int minDepth(TreeNode* root) {
    if(root == NULL) return 0;

    if(root->left == NULL && root -> right == NULL) return 1; 

    int left = minDepth(root->left);
    int right = minDepth(root->right);

  //   if(root->left == NULL) return right + 1;
  //   if(root->right == NULL) return left + 1;
  //   return min(left, right) + 1;

    return (root->left==NULL || root->right==NULL) ? (right + left + 1) : min(left, right) + 1;
  }
~~~

##### [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

~~~cpp
public:
    bool isValidBST(TreeNode* root) {
        if(root == NULL) return true;
        
        bool left = isValidBST(root->left);
        if(prev >= root->val) return false;
        else prev = root->val;
        bool right = isValidBST(root->right);
        return left && right;
    }
private:
    long prev = LONG_MIN;
~~~

##### [572. 另一棵树的子树](https://leetcode-cn.com/problems/subtree-of-another-tree/)

~~~cpp
bool isSameTree(TreeNode* r1, TreeNode* r2){
    if(r1 == NULL && r2 == NULL) return true;
    if(r1 == NULL || r2 == NULL) return false;

    return r1 -> val == r2 -> val &&
            isSameTree(r1->left, r2->left) &&
            isSameTree(r1->right, r2->right);
}
bool isSubtree(TreeNode* root, TreeNode* subRoot) {
    if(root == NULL ) return false;
    
    return isSameTree(root, subRoot) || 
            isSubtree(root->left, subRoot) ||  isSubtree(root->right, subRoot);
}
~~~



#### 3. 路径和

##### [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

~~~cpp
public:
    int helper(TreeNode * root){
        if(root == NULL) return 0;
        int left = helper(root -> left);
        int right = helper(root -> right);
        res = max(res, left + right);
        return 1 + max(left, right);
    }

    int diameterOfBinaryTree(TreeNode* root) {
        if(root == NULL) return 0;
        helper(root);
        return res;
    }
private:
    int res = 0;
~~~

##### **[124] Binary Tree Maximum Path Sum** https://leetcode.com/problems/binary-tree-maximum-path-sum/description/ 🌟🌟🌟

~~~cpp
// 字节, 美团优选

// 这道题目和上一道很类似
int maxPathSum(TreeNode* root) {
    helper(root);
    return res;
}

int helper(TreeNode * root){
    // 求 以 root 为出发点的 最大路径和
    if(root == NULL) return 0;

    int left = helper(root->left);
    int right = helper(root->right);


    // 分三种情况:
    // (1) root->val  根节点
    // (2) root->val + max(root->left, roor->right) 根节点 + 左子树 or 右子树
    // (3) root->val + root->left + root->left 根节点 + 左子树 + 右子树
    int max_path_sum = max(root->val , 
                           max(root->val + max(left, right), 
                               root->val + left + right)))
    res = max(res,  max_path_sum);


    // 分两种情况:
    // (1) 孤立的单一节点
    // (2) 根节点 + 左右子树的最大值
    return max(root->val, max(left, right) + root->val);
}
int res = INT_MIN;

~~~

扩展： 如何打印这个路径?  参考链接： https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/solution/dfsshu-chu-lu-jing-by-adl00uonvn-go3e/



##### [112. 路径总和](https://leetcode-cn.com/problems/path-sum/)

~~~cpp
// 2021.08.11腾讯云后台开发
bool hasPathSum(TreeNode* root, int targetSum) {
    if(root == NULL) return false;
    if(root->left == NULL && root->right == NULL && targetSum == root->val) return true;

    bool left = hasPathSum(root->left, targetSum - root->val);
    bool right = hasPathSum(root->right, targetSum - root->val);

    return left || right;
}
~~~

##### [113. 路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/)

~~~cpp
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        vector<vector<int> > res;
        if(root == NULL) return res;
        vector<int > track;
        helper(root, track, res, targetSum);
        return res;
    }

    void helper(TreeNode * root, vector<int> track, vector<vector<int> >& res, int target){
        // 不满足条件: 返回
        if(root == NULL) return;
        // 做选择
        track.push_back(root->val);
        // 操作
        if(root->left == NULL && root->right == NULL && root->val == target){
            res.push_back(track);
        }
        // 递归
        helper(root->left, track, res, target - root->val);
        helper(root->right, track, res, target - root->val);
        // 撤销选择
        track.pop_back();
    }
~~~

##### [129] https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/ 🌟🌟🌟

```
int sumNumbers(TreeNode* root) {
    int res = 0;
    TreeNode * cur = root;
    vector<int> path;
    recur(cur, res, path);
    return res;
}

void recur(TreeNode * root,int& res, vector<int>& path){
    if(root == NULL) return;

    path.push_back(root->val);
    if(root -> left == NULL && root -> right == NULL){ // 叶子节点
        int pathsum = 0;
        for(auto n: path) pathsum = pathsum * 10 + n;
        res += pathsum;
    }
    recur(root->left, res, path);
    recur(root->right, res, path);
    path.pop_back();
}
```

###### [剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

~~~cpp
// 蔚来一面
void recur(TreeNode * root, vector<vector<int> >& res, vector<int>& track, int target){
    if(root == NULL) return;

    track.push_back(root->val);
    if(root->left == NULL && root->right == NULL && root->val == target){
        res.push_back(track);    
    }
    recur(root->left, res, track, target-root->val);
    recur(root->right, res, track, target-root->val);
    track.pop_back();
}

vector<vector<int>> pathSum(TreeNode* root, int target) {
    vector<vector<int> > res;
    if(root == NULL) return res;
    vector<int> track;
    recur(root, res, track, target);
    return res;
}
~~~





#### 4. 重建二叉树

##### [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

~~~cpp
TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
    if(root1 == NULL && root2 == NULL) return NULL;

    if(root1 == NULL) return root2;
    if(root2 == NULL) return root1;

    TreeNode * root = new TreeNode(root1->val + root2->val);
    root->left = mergeTrees(root1->left, root2->left);
    root->right = mergeTrees(root1->right, root2->right);
    return root;
}
~~~

##### [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

~~~cpp
// 2021.6.11腾讯IEG
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
         po = preorder;  // 前序全局
         for(int i = 0; i < inorder.size(); i++){ // 中序 map
             m_map[inorder[i]] = i;
         }
         return helper(0, 0, inorder.size()-1);
    }

    TreeNode * helper(int pre_root, int left, int right){
        // 三个参数的含义:
        // pre_root: 前序遍历的根目录下标
        //     left: 中序遍历左侧下标
        //     right: 中序遍历右侧下标
        if(left > right) return NULL; // 返回调解

        TreeNode * root = new TreeNode(po[pre_root]);
        int i = m_map[po[pre_root]];
        root->left = helper(pre_root+1, left, i-1);
        root->right = helper(pre_root+(i-left)+1, i+1, right);
        return root;
    }
private:
    vector<int> po; 
    map<int, int> m_map; 
~~~

##### [297. 二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

~~~cpp
~~~







#### 5. 最低公共祖先

##### [235] https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/  

~~~cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if(p->val > root->val && q->val > root->val){
        return lowestCommonAncestor(root->right, p, q);
    }else if(p->val < root->val && q->val < root->val){
        return lowestCommonAncestor(root->left, p, q);
    }
    return root;
}
~~~

##### [236] 二叉树的最近公共祖先 https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/  🌟🌟🌟

~~~cpp
// 蔚来一面、华为二面、字节一面

TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    // exit: 根节点为空， 或者找到 p节点 或者 q节点
    if(root == NULL || p == root || q == root) return root;
    // 递归式：从根节点的左右子树寻找p、q的节点
    TreeNode * left = lowestCommonAncestor(root->left, p, q);
    TreeNode * right = lowestCommonAncestor(root->right, p, q);
    // 判断其祖先位置
    if(left && right) return root; // 分别出现在左右子树， 则该节点为最低公共祖先
    return left ? left : right; 
}
~~~



#### 6. 二叉搜索树

##### [450. 删除二叉搜索树中的节点](https://leetcode-cn.com/problems/delete-node-in-a-bst/)





