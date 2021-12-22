

## 二分查找

二分查找的核心思想在于怎么排除一半不符合条件的元素， 让搜寻的目标出现在限定范围内

基本的题目包含：二分查找、查找插入位置、旋转数组的查找、查找第一个和最后一个

~~~cpp
// 二分查找
int search(vector<int>& nums, int target) {
    int left = 0;
    int right = nums.size()-1;

    while(left <= right){  // 等号
        int mid = left + (right - left) / 2; // 防止溢出
        if(nums[mid] == target) return mid;
        else if(nums[mid] > target){
            right = mid - 1;  // -1
        }else{
            left = mid + 1;   // + 1
        }
    }
    return -1;
}
~~~

###### [35] 搜索插入位置 https://leetcode-cn.com/problems/search-insert-position/

~~~cpp
int searchInsert(vector<int>& nums, int target) {
    int begin = 0, end = nums.size() - 1;
    while(begin <= end){
        int mid = begin + (end - begin) / 2;
        if(nums[mid] == target) return mid;
        else if(nums[mid] > target) end = mid - 1;
        else begin = mid + 1;
    }
    return begin;
}
~~~

###### [33] 搜索旋转排序数组 https://leetcode-cn.com/problems/search-in-rotated-sorted-array/   🌟🌟🌟

~~~cpp
// 核心思想是： 先和nums[begin] 进行比较确定那一段有序
//            然后判定是否在有序数组之中， 进而确定二分到哪段?

int search(vector<int>& nums, int target) {
    int begin = 0, end = nums.size() - 1;

    while(begin <= end){  // ! 等号
        int mid = begin + (end - begin) / 2; // ! 防止溢出
        if(nums[mid] == target) return mid;  
        
        if(nums[mid] >= nums[begin]){  // left is short ! >=
            if(nums[begin] <= target && target < nums[mid]) end = mid-1;
            else begin = mid + 1;
        } else {   // right is sorted
            if(nums[mid] < target && target <= nums[end]) begin = mid+1;
            else end = mid - 1;
        }
    }
    return -1;
}
~~~

######  [34] 在排序数组中查找元素的第一个和最后一个位置 https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

~~~cpp
int findFirstTarget(vector<int>& nums, int target){
    int begin = 0, end = nums.size()-1;
    while(begin <= end){
        int mid = begin + (end - begin) / 2;
        if(nums[mid] == target){
            if(mid == 0 || nums[mid-1] != target) return mid;
            else  end = mid-1;
        }
        else{
            if(nums[mid] > target) end = mid-1;
            else begin = mid + 1; 
        }
    }
    return -1;
}

int findLastTarget(vector<int>& nums, int target){
    int begin = 0, end = nums.size()-1;

    while(begin <= end){
        int mid = begin + (end - begin) / 2;
        if(nums[mid] == target){
            if(mid == nums.size()-1 || nums[mid+1] != target) return mid;
            else  begin = mid+1;
        }
        else{
            if(nums[mid] > target) end = mid-1;
            else begin = mid + 1; 
        }
    }
    return -1;
}
vector<int> searchRange(vector<int>& nums, int target) {
    if(nums.size() == 0) return {-1, -1};
    int first = findFirstTarget(nums, target);
    int last = findLastTarget(nums, target);
    return vector<int>{first, last};
}
~~~

###### [lcof 11] 旋转数组的最小数字 https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/

~~~cpp
int minArray(vector<int>& numbers) {
    int left = 0, right = numbers.size() -1;
    while(left < right){   // 这里没有等号
        int mid = left + (right - left) / 2;
        if(numbers[mid] > numbers[right]){
            left = mid + 1;
        }else if(numbers[mid] < numbers[right]){
            right = mid;
        }else{
            right -= 1;  // 这里要分三种情况的， 等于的时候 right -= 1 即可
        }
    }
    return numbers[left];
}
~~~

###### [69] x 的平方根 https://leetcode-cn.com/problems/sqrtx/   🌟🌟🌟

~~~cpp
int mySqrt(int x) {
    int low = 0, high = x;     // (1) 以 x 为上界
    while(low <= high){
        int mid = low + (high-low) / 2;

        if ((long long)mid * mid > x){   // (2) 使用 long long 防止溢出
                high = mid - 1;
        } else {
                low = mid + 1;
        }
    }
    return high;   // (3) 返回 high
}
~~~

###### [162] 寻找峰值

~~~cpp
int findPeakElement(vector<int>& nums) {
    if(nums.size() == 0) return 0;
    int l = 0, r = nums.size()-1;
    while(l < r){
        int mid = l + (r - l) / 2;
        if(nums[mid] > nums[mid+1])  r = mid;
        else l = mid + 1;
    }
    return l;
}
~~~
