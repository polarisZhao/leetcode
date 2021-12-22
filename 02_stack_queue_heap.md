# 2. 栈、堆、队列 

常见题目类型：

- 设计题就是用栈实现队列、用队列实现栈、设计最小栈
- 基于这几种数据结构来实现某些功能， 比如数据流的中位数、滑动窗口的最大值、第k个最大值
- 单调栈(队列)的应用： 比如接雨水、直方图中的最大矩形

#### (1) 常见技巧总结

- 不要对空容器(栈、堆、队列进行操作)
- 单调栈/队列的应用是一个挺重要的点。
- 有时入栈的元素时下标 index， 而不是 value，这样更方便操作。因为数组可以直接按照下标进行索引。

**栈 : 先进后出**

~~~c++
stack<int> m_stack;   m_stack.size()    m_stack.empty()    m_stack.push()    m_stack.pop()    m_stack.top()
~~~

**队列：先进先出**

~~~cpp
queue<int> m_queue;   m_queue.size()    m_queue.empty()   m_queue.push()    m_queue.pop()    m_queue.front()
~~~

**双端队列**

~~~cpp
deque<int> m_deque;    m_deque.size()    m_deque.empty()   m_deque.front()    m_deque.back()
m_deque.push_back()   m_deque.pop_back()    m_deque.push_front()   m_deque.pop_front()    
~~~

**优先队列**

实现：二叉堆， 最大(小)值 先出的完全二叉树

~~~cpp
priority_queue<int> max_heap;              // 默认构造最大堆
priority_queue<int, vector<int>, less<int> > max_heap;     // 构造最大堆:less -> max_heap
priority_queue<int, vector<int>, greater<int> > min_heap;   // 构造最小堆:greater -> min_heap
max_heap.size()    max_heap.empty()     max_heap.push()      max_heap.pop()     
max_heap.top()  // 虽然它是一个队列，但是这里去元素不是 front 方法, 而是 top 方法: 返回堆顶
~~~

#### (2) 典型例题

##### 类型一: 常规题目

###### [155] https://leetcode-cn.com/problems/min-stack/   🌟🌟

~~~cpp
// 解题思路:
// 使用辅助栈记录栈的最小值
// 当添加元素时， 将待添加元素与辅助栈栈顶元素进行比较
//     当栈顶元素小于待添加元素 -> 辅助栈添加栈顶元素
//     当栈顶元素大于待添加元素 -> 辅助栈添加待添加元素
// !! 题目默认各种操作合法:即不回对空栈进行top、min和 pop 操作，所以代码中没有进行相关判定
class MinStack {
public:
    /** initialize your data structure here. */
    MinStack() {
    }
    
    void push(int x) {
        m_stk.push(x);
        if(min_stk.empty())   min_stk.push(x);
        else min_stk.push(std::min(min_stk.top(), x));
    }
    
    void pop() {
        m_stk.pop();
        min_stk.pop();
    }
    
    int top() {
        return m_stk.top();
    }
    
    int min() {
        return min_stk.top();
    }
private:
    stack<int> min_stk;
    stack<int> m_stk;
};
~~~

###### [232] https://leetcode.com/problems/implement-queue-using-stacks/  🌟🌟

~~~cpp
// 解题思路:
// (1) 申请两个栈 data_stack 和 m_stack
// (2) 当入队(添加数据)时, 执行如下三个步骤
//      - 将 data_stack 中的数据逐个 放到 m_stack 中
//      - 将 value 添加到 m_stack 中
//      - 将 m_stack 中的数据 逐个放入到 data_stack 中 
// (3) 当出队(删除数据)时， 直接弹出 data_stack 栈顶数据即可    

private:
    stack<int> data_stack;
    stack<int> m_stack;

public:
    CQueue() {
    }
    
    void appendTail(int value) {
        while(!data_stack.empty()){
            m_stack.push(data_stack.top());
            data_stack.pop();
        }
        data_stack.push(value);
        while(!m_stack.empty()){
            data_stack.push(m_stack.top());
            m_stack.pop();
        }
    }
    
    int deleteHead() {
        if(data_stack.empty()) return -1; // 边界条件!  栈空!
      
        int x = data_stack.top();
        data_stack.pop();
        return x;
    }
~~~

###### [225] 用队列实现栈 https://leetcode-cn.com/problems/implement-stack-using-queues/  🌟🌟

~~~cpp
/** Initialize your data structure here. */
MyStack() {

}

/** Push element x onto stack. */
void push(int x) {
    if(data_queue.empty()) data_queue.push(x);
    else{
        while(!data_queue.empty()){
            temp_queue.push(data_queue.front());
            data_queue.pop();
        }
        data_queue.push(x);
        while(!temp_queue.empty()){
            data_queue.push(temp_queue.front());
            temp_queue.pop();
        }
    }
}

/** Removes the element on top of the stack and returns that element. */
int pop() {
    int x = data_queue.front();
    data_queue.pop();
    return x;
}

/** Get the top element. */
int top() {
    return data_queue.front();
}

/** Returns whether the stack is empty. */
bool empty() {
    return data_queue.empty();
}
private:
queue<int> data_queue;
queue<int> temp_queue;
~~~

###### [946] https://leetcode-cn.com/problems/validate-stack-sequences/

~~~cpp
bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
    stack<int> m_stk;
    int j = 0;
    for(int i = 0;  i < pushed.size(); i++){
        m_stk.push(pushed[i]);
        while(!m_stk.empty() && m_stk.top() == popped[j]){
            j++;
            m_stk.pop();
        }
    }
    return m_stk.empty();
}
~~~



##### 类型二： 利用容器实现某种功能

###### [215] https://leetcode-cn.com/problems/kth-largest-element-in-an-array/

~~~cpp
// 查找最大 k 个值： 用最小堆, 声明的第三个类型是 greater<int>
// 查找最小 k 个值： 用最大堆, 声明的第三个类型是 less <int>
int findKthLargest(vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int> > min_heap;

    for(int i = 0; i < nums.size(); i++){
        if(i < k) min_heap.push(nums[i]);
        else{
            if(nums[i] > min_heap.top()){
                min_heap.pop();
                min_heap.push(nums[i]);
            }
        }
    }
    return min_heap.top();
}
~~~

###### [lcci 17.14] 最小K个数 https://leetcode-cn.com/problems/smallest-k-lcci/     🌟🌟

类题 : 347  Top K Frequent Elements   --> 可以使用哈希表解决

~~~cpp
// 和 leetcode 215 几乎一样, 最小值， 用最大堆!!
vector<int> smallestK(vector<int>& arr, int k) {
    vector<int> res;
    if(arr.size() == 0 || k == 0) return res;
    priority_queue<int> max_heap;   // 最大堆

    for(int i = 0; i < arr.size(); i++){
        if(i < k) max_heap.push(arr[i]);
        else{
            if(arr[i] < max_heap.top()){
                max_heap.pop();
                max_heap.push(arr[i]);
            }
        }
    }

    while(!max_heap.empty()){
        res.push_back(max_heap.top());
        max_heap.pop();
    }
    return res;
}
~~~

###### [259] https://leetcode-cn.com/problems/find-median-from-data-stream/  🌟🌟

~~~cpp
// ！ 非常经典的一道题目， 借助数据结构，降低算法复杂度
public:
    /** initialize your data structure here. */
    MedianFinder() {
    }
    
    void addNum(int num) {
        // 确保加入之后，最大堆的元素值都是小于最小堆的  
        max_heap.push(num);
        min_heap.push(max_heap.top());
        max_heap.pop();
      
        // 平衡两个堆得元素个数
        while(min_heap.size() > max_heap.size()){
            max_heap.push(min_heap.top());
            min_heap.pop();
        }
    }
    
    double findMedian() {
        return max_heap.size() > min_heap.size() ? double(max_heap.top()) 
                                                  : (max_heap.top() + min_heap.top()) * 0.5;
    }

private:
    // 保持大顶堆的元素数目大于小顶堆得元素数目
    priority_queue <int> max_heap; // 大顶堆
    priority_queue <int, vector<int>, greater<int> > min_heap; // 小顶堆
~~~

##### 类型三： 单调栈/队列

###### [lcof 59\] https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/  

~~~cpp
// ! 使用单调的双向队列
public:
    MaxQueue() {
    }
    
    int max_value() {
        if(max_deq.size() == 0) return -1;
        return max_deq.front();
    } 
    
    void push_back(int value) {
        data.push(value);
        while(max_deq.size() && max_deq.back() < value)
            max_deq.pop_back();
        max_deq.push_back(value);
    }
    
    int pop_front() {
        if(data.size() == 0) return -1;
        int n = data.front();
   
        data.pop();
        if(max_deq.size() && max_deq.front() == n)
            max_deq.pop_front();
        return n;
    }
private:
    queue<int> data;
    deque<int> max_deq;
~~~

###### [739] https://leetcode-cn.com/problems/daily-temperatures/  

~~~cpp
vector<int> dailyTemperatures(vector<int>& T) {
    stack<int> m_stk;
    vector<int> m_vec(T.size(), 0);

    for(int i = 0; i < T.size(); i++){
        if(m_stk.empty()) m_stk.push(i);
        else{
            while(!m_stk.empty() && T[i] > T[m_stk.top()]){
                m_vec[m_stk.top()] = i - m_stk.top();
                m_stk.pop();
            }
            m_stk.push(i);
        }
    }
  
    while(!m_stk.empty()){       // 其实这几行不需要， 初始化就是 0
        m_vec[m_stk.top()] = 0;
        m_stk.pop();
    }
    return m_vec;
}
~~~

###### [239] https://leetcode-cn.com/problems/sliding-window-maximum/   🌟🌟🌟

~~~cpp
// ! 使用了单调双向队列: 这是经常考查的重点
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    vector<int> res;
    deque<int> m_deque;

    for(int i = 0; i < nums.size(); i++){
        while(!m_deque.empty() && nums[i] > nums[m_deque.back()]){
            m_deque.pop_back();
        }
        m_deque.push_back(i);    // 这里进入队列的是 index， 而不是 value
        if(i - m_deque.front() >= k) m_deque.pop_front();
        // 开始添加!
        if(i >= k-1) res.push_back(nums[m_deque.front()]);

    }
    return res;
}
~~~

###### [42] 接雨水 https://leetcode-cn.com/problems/trapping-rain-water/   🌟🌟🌟

~~~cpp
int trap(vector<int>& height) {
    if(height.size() <= 2) return 0;
    int res = 0;
    // 首先找到最高的值的下标
    int max_ind = max_element(height.begin(), height.end()) - height.begin();

    // 从左向右开始遍历：如果当前值比(之前的最大值)的小，那么可以积雨水； 否则更新当前值
    int cur = 0;
    for(int i = 1; i < max_ind; i++){
        if(height[i] < height[cur])  res += (height[cur] - height[i]);
        else cur = i;
    }

    // 从右向左开始遍历
    cur = height.size() - 1;
    for(int i = cur - 1; i > max_ind; i--){
        if(height[i] < height[cur]) res += (height[cur] - height[i]);
        else cur = i;
    }
    return res;
}
~~~



###### [84] Largest Rectangle in Histogram https://leetcode.com/problems/largest-rectangle-in-histogram/description/  🌟🌟🌟

~~~cpp
int largestRectangleArea(vector<int>& heights) {
    int cur_area = 0;
    stack<int> m_stk;
    // (1) 让最后的栈内元素都可以弹出 !
    heights.push_back(0); 

    for(int i=0;  i < heights.size(); i++){
        // (1) 当当前的值比栈顶元素小的时候， 可以计算以栈顶元素为高的最大矩形的长度
        while(!m_stk.empty() && heights[i] < heights[m_stk.top()]){
            int height = heights[m_stk.top()];  // 高
            m_stk.pop();
            // 当前的元素 和 弹出元素后的栈顶 差值为 宽
            // 注意! 弹出后可能栈为空
            int width = m_stk.empty() ? i : i - m_stk.top() - 1; 
            cur_area = max(height * width, cur_area);
        }
        m_stk.push(i);
    }
    return cur_area;
}


// 最大矩形
int maximalRectangle(vector<vector<char>>& matrix) {
    if(matrix.size() == 0 || matrix[0].size() == 0) return 0;
    int n = matrix[0].size();  

    int max_area = 0;
    vector<int> heights(n, 0);
    for(int i = 0; i < matrix.size(); i++){
        for(int j = 0; j < matrix[0].size(); j++){
            if(matrix[i][j] == '1') heights[j]++;
            else heights[j] = 0;
        }
        max_area = max(largestRectangleArea(heights), max_area);
    }
    return max_area;
}
~~~
