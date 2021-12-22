

## 回溯、贪心、分治、递归   

#### 2.1 回溯

​        回溯法又称为试探法， **但当探索到某一步时， 发现原先选择达不到目标，就退回一步重新选择，这种走不通就退回再走的技术**称为回溯法。 常见的题目有：子集、组合、全排列、括号生成、电话号码的字母组合

- 记住：传递引用、排序、去重(利用集合去重)。 这其中， 排序和去重可以用于降低复杂度

- 剪枝:

  (1) 当容器长度等于深度的时候

  (2) `find(path.begin(), path.end(), nums[i]) == path.end()`  只有不在 track 中才添加

  (3) 使用 flag 进行标记  -> vistited

- 一般传递的几个参数：res、track、depth、nums(原有数组)

- 模板

  ~~~cpp
  result = []
  def backtrack(路径, 选择列表):
      if 不满足条件: return
      
      if 满足结束条件:
          result.add(路径)
          return
        
      for 选择 in 选择列表:
          做选择
          backtrack(路径, 选择列表)
          撤销选择
  ~~~

- 思考的过程就是构建决策树的过程， 要有这个思维

###### [78. 子集] https://leetcode-cn.com/problems/subsets/

~~~cpp
vector<vector<int>> subsets(vector<int>& nums) {
    vector<vector<int> > res;   // 存储最终结果的 res
    vector<int > track;         // 回溯时候的各个子集元素
    backtrack(nums, 0, res, track);
    return res;
}

void backtrack(vector<int>& nums, int start, 
               vector<vector<int> >& res, 
               vector<int>& track){
    res.push_back(track);
    for(int i = start; i < nums.size(); i++){
        track.push_back(nums[i]);  // 做选择
        backtrack(nums, i+1, res, track);  // 回溯
        track.pop_back();  // 撤销选择
    }
}
~~~

> **回溯**的执行步骤如下所示：
>
> 初始化时为空集合
>
> 当第一次递归时， 分别添加 [1]、[2]、[3] 三个集合
>
> 当第二次递归时， 对于集合 [1]， 添加 [2] 组成 [1, 2]， 添加 [3]  组成 [1, 3]
>
> ​                                对于集合 [2]， 添加 [3] 组成 [2, 3]
>
> 第三次递归时， 对于集合 [1, 2]， 添加 [3] 组成 [1, 2, 3] 即可

回溯模板

~~~python
result = []
def backtrack(路径, 选择列表):
    if 不满足条件: return
    
    if 满足结束条件:
        result.add(路径)
        return
      
    for 选择 in 选择列表:
        做选择
        backtrack(路径, 选择列表)
        撤销选择
~~~

###### [90. 子集 II] https://leetcode-cn.com/problems/subsets-ii/

~~~cpp
vector<vector<int>> subsetsWithDup(vector<int>& nums) {
    vector<vector<int> > res;
    vector<int> track;
    sort(nums.begin(), nums.end());  // 预处理：排序
    backtrack(nums, 0, res, track);
    return res;
}

void backtrack(vector<int> nums, int start,
               vector<vector<int> >& res,
               vector<int >& track){

   if(find(res.begin(), res.end(), track) == res.end()) res.push_back(track);  // 剪枝， 排除重复元素

   for(int i = start; i < nums.size(); i++){
       track.push_back(nums[i]);
       backtrack(nums, i+1, res, track);
       track.pop_back();
   }
}
~~~

###### [46] 全排列 https://leetcode-cn.com/problems/permutations/  🌟🌟🌟 🌟

~~~cpp
vector<vector<int>> permute(vector<int>& nums) {
    vector<vector<int> > res;
    vector<int> track;
    int n = nums.size();
    trackback(track, res, 0, nums);
    return res;
}

void trackback(vector<int>& track, 
               vector<vector<int> >& res,
               int depth,
               vector<int> nums
              ){
    int n = nums.size();
    if(depth == n) res.push_back(track);

    for(int i = 0; i < n; i++){
        if(find(track.begin(), track.end(), nums[i]) == track.end()){ // 防止出现重复元素
            track.push_back(nums[i]);
            trackback(track, res, depth+1, nums);
            track.pop_back();

        }
    }
}
~~~

###### [47] 全排列 II https://leetcode-cn.com/problems/permutations-ii/   🌟🌟🌟 🌟

~~~cpp
void backtrack(vector<int>& nums, vector<int>& track, vector<vector<int>>& res, vector<bool> & flag) {
    if (track.size() == nums.size() &&  
        find(res.begin(), res.end(), track) == res.end()) {
        res.push_back(track);
        return;
    }

    for (int i = 0; i < nums.size(); ++i) {
        // 剪枝 !!
        if(i > 0 && nums[i] == nums[i-1] && flag[i-1]){
            continue;
        }

        if(flag[i] == false){
            track.push_back(nums[i]);
            flag[i] = true;
            backtrack(nums, track, res, flag);
            flag[i] = false;
            track.pop_back();
        }
    }
}

vector<vector<int>> permuteUnique(vector<int>& nums) {
    vector<vector<int>> res;
    vector<int> track;
    sort(nums.begin(), nums.end());
    vector<bool> flag(nums.size(), false);
    backtrack(nums, track, res, flag);
    return res;   
}
~~~

###### [lcof] 剑指 Offer 38. 字符串的排列 https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/

~~~cpp
    void trackback(string s, int depth, string track, 
                   vector<bool> flag, set<string>& res){
        if(depth == s.size()){
            res.insert(track);
            return;
        }
        for(int i = 0; i < s.size(); i++){
            if(!flag[i]){
                track.push_back(s[i]);
                flag[i] = true;
                trackback(s, depth+1, track, flag, res);
                track.pop_back();
                flag[i] = false;
            }
        }
    }
    vector<string> permutation(string s) {
        set<string> res; // 使用 set 来降低复杂度
        string track;
        int depth = 0;
        vector<bool> flag(s.size(), 0); // 使用 flag 来标记是否遍历过
        trackback(s, depth, track, flag, res);
        return vector<string>(res.begin(), res.end());
    }
~~~

###### [22] 括号生成 https://leetcode-cn.com/problems/generate-parentheses/

~~~cpp
vector<string> generateParenthesis(int n) {
    vector<string> res;
    string str;
    backtrace(n, 0, 0, res, str);   // 已经放置了 0 个 left 和 0 个 right
    return res;
}

void backtrace(int n, int left, int right, vector<string>& res, string str){
    // 当右括号大于左括号 或者 左括号大于n 或者 右括号大于n 的时候，不符合条件 -> 剪枝
    if(right > left || right > n || left > n) return;
    // 当 左括号数目 == 右括号数目， 且左括号 = n 时， 满足条件 -> 保存
    if(right == left && left == n){
        res.push_back(str);
        return;
    }
    // 递归进行
    backtrace(n, left+1, right, res, str + '(');
    backtrace(n, left, right+1, res, str + ')');
}
~~~

###### [17] 电话号码的字母组合https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/

~~~cpp
map<int, string> m_map = {
    {2,"abc"}, {3,"def"}, {4,"ghi"}, {5,"jkl"},
    {6, "mno"}, {7,"pqrs"}, {8,"tuv"}, {9,"wxyz"}
};

vector<string> letterCombinations(string digits) {
    vector<string> res;
    if(digits.size() == 0) return res;
    string str = "";
    trackback(res, 0, digits, str);
    return res;

}       

void trackback(vector<string>& res, int depth, string digits, string& str){
    // res 存储对应的 结果
    // depth 不断加深树的深度 digits
    // 利用 depth & digits 可以获取当前的选择[这里是某个字符]
    // str 是遍历处的字符串值
    if(str.size() == digits.size()) res.push_back(str);
    string choose = m_map[digits[depth] - '0'];

    for(int i = 0; i < choose.size(); i++){
       str += choose[i]; // 选择
       trackback(res, depth+1, digits, str); // 回溯
       str.erase(str.end() - 1); // 撤销  
    }
}
~~~

###### [39] 组合总和 https://leetcode-cn.com/problems/combination-sum/

~~~cpp
void trackback(vector<vector<int>>& res, vector<int>& track, int start, vector<int>& candidates, int target){
    int s = accumulate(track.begin(), track.end(), 0);
    if(s > target) return;

    if(s == target)  res.push_back(track);

    for(int i = start; i < candidates.size(); i++){
        track.push_back(candidates[i]);
        trackback(res, track, i, candidates, target);
        track.pop_back();
    }
    
}
vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
    vector<vector<int> > res;
    vector<int> track;
    trackback(res, track, 0, candidates, target);
    return res;
}
~~~

###### [77] 组合 https://leetcode-cn.com/problems/combinations/

~~~cpp
vector<vector<int>> combine(int n, int k) {
    vector<vector<int> > res;
    vector<int> track;
    trackback(res, track, 0, n, k);
    return res;
}
void trackback(vector<vector<int> >& res, vector<int> & track, int start, int n, int k){
    if(track.size() > k) return;

    if(track.size() == k) res.push_back(track);

    for(int i = start; i < n; i++){
        track.push_back(i+1);
        trackback(res, track, i+1, n, k);
        track.pop_back();
    }
}
~~~

###### [79] 单词搜索 https://leetcode-cn.com/problems/word-search/

~~~cpp
int bfs(int r, int c, int idx){
    // 判断非法退出条件
    if(r < 0 || r >= m || c < 0 || c >= n 
       || g_word[idx] != g_board[r][c]) return false;
    // 判定符合条件
    if(idx == g_word.size() - 1) return true;

   // 四个方向的回溯
    char t = g_board[r][c];
    g_board[r][c] = '$';
    if(bfs(r-1, c, idx+1)
      ||bfs(r+1, c, idx+1)
      || bfs(r, c-1, idx+1)
      || bfs(r, c+1, idx+1)) return true;
    g_board[r][c] = t;
    return false;
}

bool exist(vector<vector<char>>& board, string word) {
    g_board = board;
    g_word = word;

    m = board.size();
    n = board[0].size();

    for(int i = 0; i < m; i++){
        for(int j = 0; j < n; j++){
            if(bfs(i, j, 0)) return true;
        }
    }
    return false;
}
int m, n;
vector<vector<char> > g_board;
string g_word;
~~~

###### [51] N 皇后 https://leetcode-cn.com/problems/n-queens/

~~~cpp
bool isValid(vector<string>& board, int row, int col, int n){
    if(row < 0 || row >= n || col < 0 || col >= n) return false;
    for(int i = 0; i < row; i++){
        if(board[i][col] == 'Q') return false;
    }
    for(int i = row-1, j = col+1; i >= 0 && j < n;i--, j++){
        if(board[i][j] == 'Q') return false;
    }
    for(int i = row-1, j = col-1; i >= 0 && j >= 0;i--, j--){
        if(board[i][j] == 'Q') return false;
    }
    return true;
}

void backtrack(vector<string>& board, int row, vector<vector<string>>& res, int n){
    if(row == board.size()){
        res.push_back(board);
        return;
    }

    // 棋盘的遍历: 列遍历是通过for 循环实现的， 行遍历是通过递归实现的
    for(int col = 0; col < n; col++){
        if(!isValid(board, row, col, n)) continue;
        board[row][col] = 'Q';
        backtrack(board, row+1, res, n);
        board[row][col] = '.';
    }

}

vector<vector<string>> solveNQueens(int n) {
    vector<vector<string>> res;
    vector<string> board(n, string(n, '.'));
    backtrack(board, 0, res, n);
    return res;
}
~~~

###### [37] 解数独 https://leetcode-cn.com/problems/sudoku-solver/

~~~cpp
bool isValid(vector<vector<char>>& board, int row, int col, int val){
    for(int i = 0; i < 9; i++)  if(board[row][i] == val) return false; // 判断行是否有重复
    for(int i = 0; i < 9; i++)  if(board[i][col] == val) return false; // 判断列是否有重复

    int startRow =  row / 3 * 3;
    int startCol = col / 3 * 3;
    for(int i = startRow; i < startRow + 3; i++){
        for(int j = startCol; j < startCol + 3; j++){
            if(board[i][j] == val) return false;
        }
    }
    return true;
}

bool backtrack(vector<vector<char>>& board){
    int m = board.size();
    int n = board[0].size();

    for(int i = 0; i < m; i++){ // 遍历行
        for(int j = 0; j < n; j++){ // 遍历列
            if(board[i][j] != '.') continue; 
            for(int k = '1'; k <= '9'; k++){
                 if(isValid(board, i, j, k)){ // (i, j) 这个位置放 k 是否合适
                    board[i][j] = k; // 放置 k
                    if(backtrack(board)) return true;  // 找到一组合适的就立刻返回
                    board[i][j] = '.'; // 回溯， 撤销k
                 }
            }
            return false; // 9个数字都尝试完了，没有合适的，返回 false
        }
    }
    return true; // 填充完毕, 没有返回 false， 说明找到合适棋盘位置了
}

void solveSudoku(vector<vector<char>>& board) {
    backtrack(board);
}
~~~



#### 2.2 贪心

**核心要点**

​    **使用贪心算法需要满足贪心选择的性质， 简单说就是：每一步都做出一个局部最优的选择，最终的结果就是全局最优。**

最关键的是分析出第一步如何是最优解

##### (1) 类型一

###### 多数元素  🌟🌟🌟

~~~cpp
// 摩尔投票法：有最优解的思想在里面
int majorityElement(vector<int>& nums) {
    int cur = nums[0];
    int cnt = 1;
    for(int i = 1; i < nums.size(); i++){
        if(cur == nums[i]) cnt++;
        else{
            cnt--;
            if(cnt == 0){
                cur = nums[i];
                cnt = 1;
            }
        }
    }
    return cur;
}
~~~

##### (2) 类型二

###### 买卖股票的最佳时机   🌟🌟🌟

~~~cpp
// 第i天的收益为 profit = prices[i] - prices[i-1]
//（1）当 profit > 0 时，当天买入卖出
//（2）当 profit <= 0 时，当天不进行交易
int maxProfit(vector<int>& prices) {
    int profit = 0;
    if(prices.size() <= 1) return 0;
    for(int i = 1; i < prices.size(); i++){
        if(prices[i] > prices[i-1])
            profit += (prices[i] - prices[i-1]);
    }
    return profit;
}
~~~

###### [121] https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/

~~~cpp
int maxProfit(vector<int>& prices) {
    int max_profit = 0;
    int min_val = INT_MAX;
    
    for(int i = 0; i < prices.size(); i++){
        min_val = min(min_val, prices[i]); // 保存之前的最小值
        max_profit = max(max_profit, prices[i] - min_val);  // 当前与最小值的差
    }
    return max_profit;
}
~~~

###### 加油站

~~~cpp
int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
    int min_spare = INT_MAX; // 最少剩余油量
    int pos = 0;
    int spare = 0; // 当前剩余油量

    // 环绕一圈， 最终油量大于等于零即可实现绕环行驶，标记最少的位置
    for(int i = 0; i < gas.size(); i++){
        spare += (gas[i] - cost[i]);
        if(spare < min_spare){
            min_spare = spare;
            pos = i; // 标记油量最少的位置
        }
    }
    return spare < 0 ? -1 : (pos + 1) % gas.size();
}
~~~

##### (3) 类型三

###### [455] 分发饼干 https://leetcode-cn.com/problems/assign-cookies/

~~~cpp
// 尽量先满足胃口值小的孩子，因为这样的孩子容易满足。
// 尽可能选用尺寸小的，这样大尺寸饼干可以用来满足胃口值大的孩子。

int findContentChildren(vector<int>& g, vector<int>& s) {
    if(s.size() == 0 || g.size() == 0) return 0;

    sort(g.begin(), g.end());
    sort(s.begin(), s.end());

    int i = 0;
    for(int j = 0; i < g.size() && j < s.size(); ++j){
        if(s[j] >= g[i]) i++; 
    }

    return i > g.size() ? g.size() : i;
}
~~~

###### [55] 跳跃游戏 [45] 跳跃游戏 II

~~~cpp
bool canJump(vector<int>& nums) {
    // 核心思想: 如果一个位置能够到达，那么这个位置左侧所有位置都能到达
    //          每一步都选择都选择从当前所在位置出发可以到达的最远距离
    int k = 0; // 代表当前遍历的节点最远可以跳到何位置
    for(int i = 0; i < nums.size(); i++){
        // 如果当前最远可以跳过超出 nums.size(), 则可以到达最后
        if(k > nums.size()) return true;
        // 如果 i 大于当前最远的距离，证明 当前位置不可达， 返回 false
        if(i > k) return false;
        // 更新当前最远位置
        k = max(k, i + nums[i]);
    }
    return true;
}
~~~

~~~cpp
int jump(vector<int>& nums) {
    // 挨个跳跃就行
    int res = 0; // 跳跃次数
    int max_pos = 0; // 标记最大位置
    int end = 0; // 最后的跳跃位置
    for(int i = 0; i < nums.size()-1; i++){
        max_pos = max(nums[i] + i, max_pos); 
        if(i == end){
            end = max_pos;
            res++;
        }
    }
    return res;
}
~~~

###### [402] 移掉K位数字 https://leetcode-cn.com/problems/remove-k-digits/

~~~cpp
// 用 string 来表示一个单调栈:
//   当前数字大于最后一个时候， 将最后一个字母弹出，并k计数减一
// 当前数字追加
// 
string removeKdigits(string num, int k) {
    string res; // 默认是一个空串
    int n = num.size(), m = n - k;
    for(char c:num){
        while(k && res.size() && res.back() > c){
            res.pop_back();
            --k;
        }
        res.push_back(c);
    }
    res.resize(m); // 调整大小
    res.erase(0, res.find_first_not_of("0")); // 去除前导零
    return res.empty() ? "0" : res; // "0" 错误
}
~~~

##### (4) 类型四

- 可能需要进行排序作为预处理
- 一般对排序后的容器进行遍历， 贪心的找出局部最优解

###### [435] 无重叠区间

~~~cpp
int eraseOverlapIntervals(vector<vector<int>>& intervals) {
    if(intervals.size() < 1) return 0;

    // 排序
    sort(intervals.begin(), intervals.end(), [](vector<int> a, vector<int> b){return a[1] < b[1];});
    int cnt = 0;

    int tail = intervals[0][1];
    // 贪心的每一步: 选择未被排除的区间中 [右区间 坐标最小] 的那个作为选定区间
    for(int i = 1; i < intervals.size(); i++){
        // 如果有交集， 去掉该区间
       if(intervals[i][0] < tail) cnt++;
        // 没有交集，选择该区间
       else tail = intervals[i][1];
    }
    return  cnt;
} 

~~~

###### [646]. 最长数对链 https://leetcode-cn.com/problems/maximum-length-of-pair-chain/)

~~~cpp
// 还是按照 区间末尾进行排序!!
int findLongestChain(vector<vector<int>>& pairs) {

    sort(pairs.begin(), pairs.end(), 
            [](vector<int> a, vector<int> b){
                return a[1] < b[1];
            });

    int res = 0;
    int i = 0;
    while(i < pairs.size()){
        int j = i + 1;
        while(j < pairs.size() && pairs[i][1] >= pairs[j][0]) j++;
        i = j;
        res += 1;
    }
    return res;
}
~~~

###### [452] 用最少数量的箭引爆气球 https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons/

~~~cpp
// 此题和上一道题目思路相似
int findMinArrowShots(vector<vector<int>>& points) {
    if(points.size() < 1) return 0;

    sort(points.begin(), points.end(), [](vector<int> a, vector<int> b){return a[1] < b[1];});

    int cnt = 1;
    int tail = points[0][1];
    for(int i = 1; i < points.size(); i++){
        // 当 左区间 大于 这个区间的右区间时， 需要一根弓箭
        if(points[i][0] > tail){
            tail = points[i][1];
            cnt++;
        }
    }
    return cnt;
}
~~~

###### [253] 会议室 II https://leetcode-cn.com/problems/meeting-rooms-ii/  🌟🌟🌟

~~~cpp
int minMeetingRooms(vector<vector<int>>& intervals) {
    // 按照开始时间排序
    sort(intervals.begin(), intervals.end());
    // 最小堆， 用来存储room 的结束时间
    priority_queue<int, vector<int>, greater<int> >rooms;
    // 
    for(int i = 0; i < intervals.size(); i++){
        // 如果堆为空， 则直接插入
        if(rooms.empty()) rooms.push(intervals[i][1]); 
        // 堆不空
        else{
            // 如果开始时间大于最小的结束时间， 直接替换掉堆顶即可
            if(rooms.top() <= intervals[i][0])
                rooms.pop();
            // 否则再开辟一个新的房间
            rooms.push(intervals[i][1]);
        }
    }
    return rooms.size();
}
~~~

###### 根据身高重建队列

~~~cpp
vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
    // 先排序
    // [7,0], [7,1], [6,1], [5,0], [5,2], [4,4]
    sort(people.begin(), people.end(), 
            [](vector<int> a, vector<int>b)
            {return (a[0] > b[0]) || (a[0] == b[0] && a[1] < b[1]);});

    // 再一个一个插入, 插入位置为第二个元素的大小
    // [7,0]
    // [7,0], [7,1]
    // [7,0], [6,1], [7,1]
    // [5,0], [7,0], [6,1], [7,1]
    // [5,0], [7,0], [5,2], [6,1], [7,1]
    // [5,0], [7,0], [5,2], [6,1], [4,4], [7,1]
    list<vector<int>> li;
    for(auto elem:people){
        auto pos = li.begin(); // 找到列表头
        advance(pos, elem[1]);   // 移动 elem[1]
        li.insert(pos, elem);
    }
    return vector<vector<int> >(li.begin(), li.end());
}
~~~

###### [56] 合并区间 https://leetcode-cn.com/problems/merge-intervals/   🌟🌟🌟

~~~cpp
vector<vector<int>> merge(vector<vector<int>>& intervals) {
    vector<vector<int> > res;
    if(intervals.size() == 0) return res;

    // !! 按照左端点进行排序: 方便后序处理 有交集并且是包含关系的情况
    sort(intervals.begin(), intervals.end(), 
        [](vector<int> a, vector<int>b){ return a[0] < b[0];});

    int cur = 0;
    res.push_back(intervals[0]);

    // 需要考虑两种情况:(1) 有无交集 & 是否是包含关系
    for(int i = 1; i < intervals.size(); i++){
        if(intervals[i][0] > res.back()[1]){  // 无交集
            res.push_back(intervals[i]);
        }else if(intervals[i][1] > res.back()[1]){  // 有交集 且 不是包含关系
            res.back()[1] = intervals[i][1];
        }
    }
    return res;
}
~~~
