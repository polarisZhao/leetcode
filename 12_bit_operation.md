## 位运算  

![](../z_img/leetcode-algorithm/bit.png)

#### (1) 常见技巧总结

1. 等号的优先级是大于位运算的!  n & (n-1) == 0 等价于 n & ((n-1) == 0) 而不是 (n & (n-1)) == 0

2. 对 32bit 位分别进行运算是一种思想 !

~~~cpp
// &   与    |   或    ~  非      ^   异或
// 最常见的几个用法
// (1) 
n & (n-1)  // 将二进制 n 的最后一位 由 1 变成 0
n ^ (n & (n-1)) // 只保留最后一个 bit 位， 将 n = 1110 变为 n = 0010
// (2) 
for(auto n:nums) ret ^= num;  // 利用 n ^ n = 0 进行去重  
// (3) 取位和置位
n |= 1 << bit	// 置位操作: 将 n 的 bit 位设置为 1, 1 << bit 产生一个只有 bit 位为 1， 其他均为 0 的数
n &= ~(1 << bit) // 置位操作， 将 n 的 bit 位置 0
n & 1 << bit   // 取位操作: 判断 n 的 bit 位是否是 1
~~~

#### (2) 典型例题

##### ① 使用位运算替换 +、>、swap 操作

###### [371] https://leetcode-cn.com/problems/sum-of-two-integers/

~~~cpp
int getSum(int a, int b) {
    while(b){
        int carry = (unsigned int)(a & b) << 1;
        a = a ^ b;
        b = carry;
    }
    return a;
}
~~~

#####  ② n & (n - 1) 将二进制 n 的最后一位 1 变成 0  

###### [191] https://leetcode-cn.com/problems/number-of-1-bits/

~~~cpp
int hammingWeight(uint32_t n) {
    int res = 0;
    while(n > 0){
        n = n & (n-1);
        res++;
    }
    return res; 
}
~~~

###### [338] https://leetcode.com/problems/counting-bits/    🌟🌟

~~~cpp
// i & (i - 1) 可以去掉 i 最右边的一个1(如果有)，
// 所以 i 的1的个数就是 i & (i - 1)的1的个数加上1
vector<int> countBits(int num) {
    vector<int> dp(num + 1, 0);
    for(int i = 1; i <= num; i++){
        dp[i] = dp[i & (i-1)] + 1;
    }
    return dp;
}
~~~

###### [461] https://leetcode-cn.com/problems/hamming-distance/

~~~cpp
// 求异或结果的 1 的个数
int hammingDistance(int x, int y) {
    int cnt = 0, n = x ^ y;
    while(n){
        n &= (n-1);
        cnt++;
    }
    return cnt;
}
~~~

###### [477] https://leetcode-cn.com/problems/total-hamming-distance/

~~~cpp
// !! 对 32 bit 的每一位进行处理，这是一种思想
int totalHammingDistance(vector<int>& nums) {
    int res = 0;
    for(int i = 0; i < 32; i++){
        int cnt = 0;
        for(auto n:nums) if(n & (1 << i)) cnt += 1;
        res += (cnt) * (nums.size() - cnt);
    }
    return res;
}
~~~

###### [231] https://leetcode-cn.com/problems/power-of-two/  🌟🌟

~~~cpp
// 如果一个数是2的幂，那么其二进制中只有一位数为 1
bool isPowerOfTwo(int n) {
    if(n <= 0) return false;
    return (n & (n-1)) == 0;
}
~~~

###### [326] https://leetcode-cn.com/problems/power-of-three/

~~~cpp
bool isPowerOfThree(int n) {
    if(n < 3) return false;
    while( n % 3 == 0){
        n /= 3;
    }
    return n == 1;
}
~~~

###### [342] https://leetcode-cn.com/problems/power-of-four/

~~~cpp
// !!! 等号的优先级是要大于 位运算的
// power of four的特点是其二进制表示除了只有1位为1外，
// 其二进制为1的数在奇数数位，所以可以根据 num & 0xaaaaaaaa 来获取结果
bool isPowerOfFour(int num) {
    return (num > 0) && (num & (num - 1)) == 0 && (num & 0xaaaaaaaa) == 0; 
}
~~~

##### ③ 异或运算  n ^ n, 利用 n ^ n = 0 来去重

~~~cpp
// 常见用法
for(auto n:nums) ret ^= num;
~~~

###### [136] https://leetcode.com/problems/single-number/      🌟🌟

~~~cpp
int singleNumber(vector<int>& nums) {
    int res = 0;
    for(auto n:nums) res ^= n;
    return res;
}
~~~

###### [389] https://leetcode.com/problems/find-the-difference/

~~~cpp
char findTheDifference(string s, string t) {
    char res = t[0];
    for(int i = 0; i < s.size(); i++) res ^= s[i];
    for(int i = 1; i < t.size(); i++) res ^= t[i];
    return res;
}
~~~

###### [268] https://leetcode.com/problems/missing-number/ 

~~~cpp
int missingNumber(vector<int>& nums) {
    int len = nums.size()， res = 0;
    for(int i = 0; i <= len; i++)   res ^= i;
    for(int i = 0; i < len; i++)  res ^= nums[i];
    return res;
}

// 解法二: 求和 0-n, 然后减去数组中所有的数
int missingNumber(vector<int>& nums) {
    if(nums.size() == 0) return 0;

    int res = 0;
    for(int i = 0; i <= nums.size(); i++) res ^= i;
    for(int i = 0; i < nums.size(); i++) res ^= nums[i];

    return res;
}
~~~

###### [260] https://leetcode-cn.com/problems/single-number-iii/     🌟🌟

~~~cpp
vector<int> singleNumbers(vector<int>& nums) {
    int diff = 0;
    for(auto n:nums) diff ^= n;

    int last_diff = (diff & (diff - 1)) ^ diff;

    int resA = 0, resB = 0;
    for(int i = 0; i < nums.size(); i++){
        if(nums[i] & last_diff) resA ^= nums[i];
        else resB ^= nums[i];
    }
    return vector<int> {resA, resB};
}
~~~

###### [169] https://leetcode-cn.com/problems/majority-element/   🌟🌟

~~~cpp
// 没有用到 位运算， 但是大部分题目都会将其放到位运算这个类别当中
// 解法二: 摩尔投票法
int majorityElement(vector<int>& nums) {
    int cur = nums[0];
    int cnt = 1;
    for(int i = 1; i < nums.size(); i++){
        if(nums[i] != cur){
            --cnt;
            if(cnt <= 0){
                cur = nums[i];
                cnt = 1;
            }
        }else{
            cnt += 1; 
        }
    }
    return cur;
}
~~~

##### ④  取位和 置位操作

~~~cpp
 n |= 1 << bit	// 置位操作: 将 n 的 bit 位设置为 1, 1 << bit 产生一个只有 bit 位为 1， 其他均为 0 的数
 n &= ~(1 << bit) // 置位操作， 将 n 的 bit 位置 0
 n & 1 << bit   // 取位操作: 判断 n 的 bit 位是否是 1
~~~

###### [190] https://leetcode.com/problems/reverse-bits/

~~~cpp
// A & (1 << i)  获取 i 位的 bit
// A |= (1 << i)  将 i 位 设置为 bit
uint32_t reverseBits(uint32_t n) {
    uint32_t res = 0;
    for(int i = 0; i < 32; i++){
        bool bit = (1 << i) & n; // get
        res |= (bit << (32 - i - 1)); // set
    }
    return res;
}
~~~

###### [137] https://leetcode.com/problems/single-number-ii/

~~~cpp
int singleNumber(vector<int>& nums) {
    int res = 0;
    for(int i = 0; i < 32; i++){
        int cnt = 0;
        for(auto n:nums) if((1 << i) & n) cnt+=1;
        if(cnt % 3 == 1) res += (1 << i);
    }
    return res;
}
~~~

##### ⑤ 其他

###### [89] https://leetcode-cn.com/problems/gray-code/

~~~cpp
vector<int> grayCode(int n) {
    int max = pow(2, n);
    vector<int> res;
    for(int i = 0; i < max; i++){
        res.push_back((i >> 1) ^ i); // 格雷码运算
    }
    return res;
}
~~~

###### [201] https://leetcode-cn.com/problems/bitwise-and-of-numbers-range/

~~~cpp
int rangeBitwiseAnd(int m, int n) {
    int res = n;
    for(int i = m; i < n & res != 0; i++){  // 添加一个 res != 0 防止超时
        res &= i;
    }
    return res;
}
~~~


### 4. 数学和归纳总结

###### [7] 整数反转 https://leetcode-cn.com/problems/reverse-integer/

~~~cpp
int reverse(int x) {
    long res = 0;
    // 可以处理负数的情况
    while(x != 0){
        res = res * 10 + x % 10;
        x /= 10;
    }
    if(res > INT_MAX || res < INT_MIN) return 0;
    return res;  
}
~~~

###### [lcof 65] https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/

~~~cpp
int add(int a, int b) {
    while(b){
        int c_in = (unsigned int) (a & b) << 1; // 进位
        a = a ^ b; // 相加
        b = c_in; // 赋值
    }
    return a;
}
~~~

###### [292] Nim 游戏 https://leetcode-cn.com/problems/nim-game/

~~~cpp
bool canWinNim(int n) {
    // 巴什博奕，n % (m+1) != 0 时，先手总是会赢的
    return n % 4 != 0;
}
~~~

###### [50]. Pow(x, n)] https://leetcode-cn.com/problems/powx-n/   🌟🌟🌟

~~~cpp
double myPow(double x, int n) {
    double res = 1.0;
    // 录用快速幂进行解题:
    //  pow = x^n = 
    //    (x^2)^(n//2), n为偶数
    //   x(x^2)^(n//2), n为奇数
    for(int i = n; i != 0; i /= 2){
        if(i % 2 != 0){ // 这里用 i % 2 ！= 0 可以同时处理 正负两种情况!
            res *= x;
        }
        x *= x;
    }
    return n > 0 ? res : 1.0 / res;
}
~~~