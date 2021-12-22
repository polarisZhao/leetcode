## 数组和字符串   

##### 题型一: 同向双指针

###### [283] 移动零 https://leetcode-cn.com/problems/move-zeroes/   🌟🌟

~~~cpp
// 经典双指针
void moveZeroes(vector<int>& nums) {
    int i = 0;
    for(int j = 0; j < nums.size(); j++){
        if(nums[j] != 0){   // 当不为零的时候，
            nums[i] = nums[j];   // 直接进行替换
            i++;
        }
    }
    while(i < nums.size()) nums[i++] = 0; 
}
~~~

###### [27] 移除元素 https://leetcode-cn.com/problems/remove-element/

~~~cpp
int removeElement(vector<int>& nums, int val) {

    int  i = 0;
    for(int j = 0; j < nums.size(); j++){
        if(nums[j] != val){
            nums[i] = nums[j];
            i++;
        }
    }
    return i;
}
~~~

###### [26]. 删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

~~~cpp
int removeDuplicates(vector<int>& nums) {
    if(nums.size() == 0) return 0;

    int i = 0;
    for(int j = 0; j < nums.size(); j++){
        if(nums[i] != nums[j]){
            i++;
            nums[i] = nums[j];
        }
    }
    return i+1;
}
~~~

###### [80] 删除排序数组中的重复项 II https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/

~~~cpp
int removeDuplicates(vector<int>& nums) {
    if(nums.size() <= 2) return nums.size();

    int p = 1;
    int cnt = 1;
    for(int i = 1; i < nums.size(); i++){
        if(nums[i] == nums[i-1]) cnt++;
        else cnt = 1;  

        if(cnt <= 2) nums[p++] = nums[i];
    }
    return p;
}
~~~

###### [58] 最后一个单词的长度 https://leetcode-cn.com/problems/length-of-last-word/

~~~cpp
int lengthOfLastWord(string s) {
    int res = 0;

    int pos = s.size() - 1;
    while(pos >= 0 && s[pos] == ' ') pos--;
    while(pos >= 0 && s[pos--] != ' ') res++;
    
    return res;
}
~~~

###### [lcof 21] 调整数组顺序使奇数位于偶数前面 https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

~~~
vector<int> exchange(vector<int>& nums) {
    int i = 0, j = nums.size() - 1;
    while(i < j){
        while(i <j && nums[i] % 2 == 1) i++;
        while(i < j && nums[j] % 2 == 0) j--;
        swap(nums[i++], nums[j--]);
    }
    return nums;
}
~~~



##### 题型二: 相向双指针

###### [167] 两数之和 II - 输入有序数组 https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/    🌟🌟

~~~cpp
vector<int> twoSum(vector<int>& numbers, int target) {
    int begin = 0, end = numbers.size() - 1;
    while(begin < end){
        int s = numbers[begin] + numbers[end];
        if(s == target) return {begin+1, end+1};
        else if(s > target) end--;
        else begin++;
    }
    return {-1, -1};
}
~~~

###### [15] 三数之和 https://leetcode-cn.com/problems/3sum/   🌟🌟

~~~cpp
vector<vector<int>> threeSum(vector<int>& nums) {
    set<vector<int> >  res;  // set 去重
    if(nums.size()==0) return {};
    sort(nums.begin(), nums.end());

    for(int i = 0; i < nums.size() - 1; i++){
        if(nums[i] > 0) continue;  // 剪枝
        if(i > 0 && nums[i] == nums[i-1]) continue;

        int l = i + 1, r = nums.size()-1;
        while(l < r){
            if(nums[l] + nums[r] == -1 * nums[i]){
                res.insert({nums[i], nums[l], nums[r]});
                l++;
                r--;
            }else if(nums[l] + nums[r] > -1 * nums[i]){
                r--;
            }else{
                l++;
            }
        }
    }
    return vector<vector<int>>(res.begin(), res.end());
}
~~~

###### [18] 四数之和 https://leetcode-cn.com/problems/4sum/

~~~ cpp
vector<vector<int>> fourSum(vector<int>& nums, int target) {
    set<vector<int> > res;
    if(nums.size() < 3) return {};

    sort(nums.begin(), nums.end());

    int n = nums.size();
    for(int i = 0; i < n - 3; i++){
        for(int j = i + 1; j < n-2; j++){
            int t = target - (nums[i] + nums[j]);
            int l = j + 1, r = n-1;
            while(l < r){
                if(nums[l] + nums[r] == t){
                    res.insert({nums[i], nums[j], nums[l], nums[r]});
                    l++;
                    r--;
                }else if(nums[l] + nums[r] > t){
                    r--;
                }else{
                    l++;
                }
            }
        }
    }
    return vector<vector<int> >(res.begin(), res.end());
}
~~~

###### [16] 最接近的三数之和 https://leetcode-cn.com/problems/3sum-closest/

~~~cpp
int threeSumClosest(vector<int>& nums, int target) {
    int res = 0;
    int min_diff  = INT_MAX;
    
    sort(nums.begin(), nums.end());
    int n = nums.size();
    for(int i = 0; i < n-2; i++){
        int l = i+1, r = n-1;
        while(l < r){
            int s = nums[i] + nums[l] + nums[r];
            if(abs(s - target) < min_diff){
                min_diff = abs(s - target);
                res = s;
            }
            if(s == target){
                l++;r--;
            }else if(s > target){
                r--;
            }else{
                l++;
            }
        }
    }
    return res;
}
~~~

###### [11] 盛最多水的容器 https://leetcode-cn.com/problems/container-with-most-water/

~~~cpp
int maxArea(vector<int>& height) {
    int l = 0, r = height.size()-1;
    int max_water = 0;
    while(l < r){
        int water = (r-l) * min(height[l], height[r]);
        max_water = max(max_water, water);
        if(height[l] < height[r]) l++; // 移动短板即可 !!
        else r--;
    }
    return max_water;
}
~~~



##### 题型三: 二维矩阵

###### [59]. 螺旋矩阵 II https://leetcode-cn.com/problems/spiral-matrix-ii/

~~~cpp
vector<vector<int>> generateMatrix(int n) {
    vector<vector<int> > matrix(n, vector<int>(n));

    int row_b = 0, row_e = n - 1;
    int col_b = 0, col_e= n - 1;

    int val = 1;
    while(row_b<= row_e && col_b <= col_e){
        for(int i = col_b; i <= col_e; i++)  matrix[row_b][i] = val++;
        row_b++;
        for(int i = row_b; i <= row_e; i++)  matrix[i][col_e] = val++;
        col_e--;
        for(int i = col_e; i >= col_b; i--)  matrix[row_e][i] = val++;
        row_e--;
        for(int i = row_e; i >= row_b; i--)  matrix[i][col_b] = val++;
        col_b++;
    }
    return matrix;
}
~~~

###### [54] 螺旋矩阵 https://leetcode-cn.com/problems/spiral-matrix/

~~~cpp
// 行列不同，可以在每一行/列 都进行一次弹出判断
vector<int> spiralOrder(vector<vector<int>>& matrix) {
    vector<int> res;
    if(matrix.size() == 0 || matrix[0].size() == 0) return res;
    int start_r = 0,  end_r = matrix.size() - 1;
    int start_c = 0, end_c = matrix[0].size() - 1;

    while(true){ // 这里退出的条件放在每个内层循环里面
        for(int i = start_c; i <= end_c; i++) res.push_back(matrix[start_r][i]);
        if(++start_r > end_r) break;
        for(int i = start_r; i <= end_r; i++) res.push_back(matrix[i][end_c]); 
        if(--end_c < start_c) break;
        for(int i = end_c; i >= start_c; i--) res.push_back(matrix[end_r][i]);
        if(--end_r < start_r) break;
        for(int i = end_r; i >= start_r; i--) res.push_back(matrix[i][start_c]);
        if(++start_c > end_c)  break;
    }
    return res;
}
~~~

###### [74] 搜索二维矩阵 https://leetcode-cn.com/problems/search-a-2d-matrix/

~~~cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    if(matrix.size() == 0 || matrix[0].size() == 0) return false;
    int raw = matrix.size(), col = matrix[0].size();

    int i = 0, j = col - 1;
    while(i < raw && j >= 0){
        if(matrix[i][j] == target) return true;
        else if(matrix[i][j] > target) j--;
        else i++;
    }
    return false; 
}
~~~

###### [48] 旋转图像 https://leetcode-cn.com/problems/rotate-image/   🌟🌟

~~~cpp
void rotate(vector<vector<int>>& matrix) {
    if(matrix.size() == 0 || matrix[0].size() == 0) return;
    int row = matrix.size(), col = matrix[0].size();

    // 沿主对角线对折
    for(int i = 0; i < row; i++){
        for(int j = i; j < col; j++){
            swap(matrix[i][j], matrix[j][i]);
        }
    }

    // 水平翻转
    for(int i = 0; i < row; i++){
        for(int j = 0; j < col / 2; j++){
            swap(matrix[i][j], matrix[i][col-1-j]);
        }
    }
    return;
}
~~~

###### [73]. 矩阵置零 https://leetcode-cn.com/problems/set-matrix-zeroes/

~~~cpp
void setZeroes(vector<vector<int>>& matrix) {
    int m = matrix.size(); // 行
    int n = matrix[0].size(); // 列

    vector<int> raw(m, 0); 
    vector<int> col(n, 0);
    for(int i = 0; i < m; i++){
        for(int j = 0;  j < n; j++){
            if(matrix[i][j] == 0) raw[i]=1;
            if(matrix[i][j] == 0) col[j]=1;
        }
    }

    for(int i = 0; i < raw.size(); i++){
        if(raw[i])
            for(int j = 0; j < n; j++)   matrix[i][j] = 0;
    }

    for(int j = 0; j < col.size(); j++){
        if(col[j])
            for(int i = 0; i < m; i++)   matrix[i][j] = 0;
    }  
}
~~~



##### 题型四: 回文与翻转

###### [9] 回文数 https://leetcode-cn.com/problems/palindrome-number/

~~~cpp
bool isPalindrome(int x) {
    string str = to_string(x);
    int begin = 0, end = str.size()-1;
    while(begin < end){
        if(str[begin++] != str[end--]) return false;
    }
    return true;

}
~~~

###### [lcof 58] https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/   🌟🌟

~~~
void reverse(string& s, int i, int j){
    while(i < j){
        swap(s[i++], s[j--]);
    }
}
string reverseLeftWords(string s, int n) {
    reverse(s, 0, n-1);
    reverse(s, n, s.size() - 1);
    reverse(s, 0, s.size() - 1);
    return s;
}
~~~

###### [557] 反转字符串中的单词 III https://leetcode-cn.com/problems/reverse-words-in-a-string-iii/

~~~cpp
void reverse(string& a, int start, int end){
    while(start < end){
        swap(a[start++], a[end--]);
    }
}
string reverseWords(string s) {
    for(int i = 0; i < s.size(); i++){
        while(i < s.size() && s[i] == ' ') i++; // 指向非空
        int j = i+1;
        while(j < s.size() && s[j] != ' ') j++;
        reverse(s, i, j-1);
        i = j;
    }
    return s;
}
~~~

###### [lcof 58] 翻转单词顺序 https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/

~~~cpp
// 全手写: 有点繁琐
string reverseWords(string s) {
    // 预处理: 去除首尾字符
    s.erase(0, s.find_first_not_of(" "));  
    s.erase(s.find_last_not_of(" ") + 1);
    if(s.empty()) return s; // 可以防止多空格的情况

    // 单词拆分
    vector<string> vec_word;
    for(int i = 0; i < s.size();){
        int j = i + 1;
        while(j < s.size() && s[j] != ' ') j++;
        vec_word.push_back(s.substr(i, j-i));
        while(j < s.size() && s[j] == ' ') j++;
        i = j;
    }

    // 连接
    string res;
    for(int i = vec_word.size()-1; i > 0; i--){
        res += vec_word[i];
        res += " ";
    }
    res += vec_word[0];
    return res;
}
~~~



##### 题型五: 字符串和数字交互

###### [8] 字符串转换整数 (atoi) https://leetcode-cn.com/problems/string-to-integer-atoi/

~~~cpp
int myAtoi(string s) {
    int pos = 0;
    while(s[pos] == ' ') pos++;

    int positive = true;
    if(s[pos] == '+'){
        positive = true;
        pos++;
    }
    else if(s[pos] == '-'){
        positive = false;
        pos++;
    }

    long res = 0;
    while(isdigit(s[pos])){
        res = res * 10 + (s[pos] - '0');
        if(res > INT_MAX || res < INT_MIN) break;
        pos++;
    }

    res = positive ? res : -1 * res;
    if(res > INT_MAX) return INT_MAX;
    if(res < INT_MIN) return INT_MIN;
    return (int)res;
}
~~~

###### 数组中重复的数字

~~~cpp
int findRepeatNumber(vector<int>& nums) {
    set<int> m_set;
    for(auto n:nums){
        if(m_set.find(n) == m_set.end())
            m_set.insert(n);
        else
            return n;
    } 
    return 0;
}
~~~

###### [lcof 66] 构建乘积数组 https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof/

~~~cpp
vector<int> constructArr(vector<int>& a) {
    if(a.size() == 0) return {};

    int len = a.size();
    vector<int> res(a.size(), a[len-1]);
    for(int i = len - 2; i >= 0; i--)
        res[i] = res[i+1] * a[i];

    int m = 1;
    for(int i = 0;  i < a.size()-1; i++){
        res[i] = m * res[i+1];
        m *= a[i];
    }
    res[a.size()-1] = m;

    return res;
}
~~~

###### [415] 字符串相加 https://leetcode-cn.com/problems/add-strings/   🌟🌟

~~~
string addStrings(string num1, string num2) {
    reverse(num1.begin(), num1.end());
    reverse(num2.begin(), num2.end());
    string res = "";
    int c_in = 0;
    int i = 0;
    while(i < num1.size() || i < num2.size()){
        int n1 = i < num1.size() ? num1[i] - '0':0;
        int n2 = i < num2.size() ? num2[i] - '0':0;
        int s = n1 + n2 + c_in;
        c_in = s / 10;
        res = res + to_string(s % 10);
        i++;
    }
    if(c_in) res += to_string(c_in);

    reverse(res.begin(), res.end());
    return res;
}
~~~

###### [14] 最长公共前缀 https://leetcode-cn.com/problems/longest-common-prefix/

~~~cpp
string longestCommonPrefix(vector<string>& strs) {
    if(strs.size() == 0) return "";
    if(strs.size() == 1) return strs[0];

    for(int i = 0; i < strs[0].size(); i++){
        char c = strs[0][i];
        for(int j = 1; j < strs.size(); j++){
            if(strs[j].size() < i) return strs[0].substr(0, i);
            if(strs[j][i] != c) return strs[0].substr(0, i);
        }
    }
    return strs[0];
}
~~~

###### [88] 合并两个有序数组 https://leetcode-cn.com/problems/merge-sorted-array/   🌟🌟

~~~cpp
void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
    nums1.resize(m+n);
    int i = m-1, j = n-1, k = m+n-1;
    while(i >= 0 && j >= 0){
        if(nums1[i] < nums2[j]) nums1[k--] = nums2[j--];
        else nums1[k--] = nums1[i--];
    }

    while(j >= 0)  nums1[k--] = nums2[j--];
}
~~~

###### [217] 存在重复元素 https://leetcode-cn.com/problems/contains-duplicate/

~~~cpp
bool containsDuplicate(vector<int>& nums) {
    set<int> m_set(nums.begin(), nums.end());
    return nums.size() > m_set.size();
}
~~~



##### 滑动窗口



~~~cpp
int lengthOfLongestSubstring(string s) {
    map<char, int> m_map;
    int i = 0;
    int max_len = 0;

    for(int j = 0; j < s.size(); j++){
        if(m_map.find(s[j]) == m_map.end() || m_map[s[j]] < i ){
            max_len = max(max_len, j-i+1); 
        }else{
            i = m_map[s[j]] + 1;
        }
        m_map[s[j]] = j;

    }
    return max_len;
} 
~~~

###### [31] 下一个排列 https://leetcode-cn.com/problems/next-permutation/

~~~cpp
void nextPermutation(vector<int>& nums) {
    // 1. 先找出最大的索引 k 满足 nums[k] < nums[k+1]，如果不存在，就翻转整个数组
    int index1 = -1;
    for(int i = nums.size()-2; i >= 0; i--){
        if(nums[i] < nums[i+1]){
            index1 = i;
            break;
        }
    }
    if(index1 == -1){
        reverse(nums, 0, nums.size()-1);
        return;
    }

    // 2. 再找出另一个最大索引 l 满足 nums[l] > nums[k]；
    int index2 = -1;
    for(int i = nums.size()-1; i >= 0; i--){
        if(nums[i] > nums[index1]){
            index2 = i;
            break;
        }
    }

    //  3. 交换 nums[l] 和 nums[k]；
    swap(nums[index1], nums[index2]);
    //  4. 最后翻转 nums[k+1:]。
    reverse(nums, index1+1, nums.size()-1); 
}

void reverse(vector<int> & nums, int start, int end){
    while(start < end){
        swap(nums[start++], nums[end--]);
    }
}
~~~

###### [3] 无重复字符的最长子串 https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)    🌟🌟🌟

~~~cpp
int lengthOfLongestSubstring(string s) {
    int res = 0;
    // 用 lookup 来替代滑动窗口， 实现高效去重查询
    unordered_set<int> lookup;
    int left = 0;
    for(int i = 0; i < s.size(); i++){
        while(lookup.find(s[i]) != lookup.end()){
            lookup.erase(s[left]);
            left++;
        }
        res = max(res, i - left + 1);
        lookup.insert(s[i]);
    }
    return res;
}
~~~