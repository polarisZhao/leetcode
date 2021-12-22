
## 排序 

主要集中在**归并排序**、**快速排序**和**堆排序**几种排序方法上

sort 函数的一些要点：

- 接口

  ~~~cpp
  template <class RandomAccessIterator>
    void sort (RandomAccessIterator first, RandomAccessIterator last);	
  template <class RandomAccessIterator, class Compare>
    void sort (RandomAccessIterator first, RandomAccessIterator last, Compare comp);
  ~~~

- 并非所有容器都使用sort算法

  - 关系型容器拥有自动排序功能，因为底层采用 rb-tree，所以不需要用到 sort 算法。
  - 序列式容器中的 stack、queue 和 priority-queue 都有特定的出入口，不允许用户对元素排序。
  - 只有 vector、deque，适用 sort 算法

  对于不能排序的容器，一般需要转化为 vector 进行排序。 比如

  ~~~cpp
  vector<pair<int, int> > pair_vec(m_map.begin(), m_map.end());
  sort(pair_vec.begin(), pair_vec.end(), 
       [](pair<int, int> a, pair<int, int> b){return a.second > b.second;}); 
  ~~~

- 底层实现： STL 的 sort 算法，数据量大时采用 QuickSort 快排算法。一旦分段后的数据量小于某个阈值(16)，为避免 QuickSort 快排的递归调用带来过大的额外负荷，就改用 Insertion Sort 插入排序。如果递归层次过深，还会改用 HeapSort 堆排序。

- sort 接受一个用户指定的函数 cmp。可以使外部定义的函数， 也可以是内嵌的 lambda 函数。 要求: 对于排序后的每两个相邻元素都要满足使 cmp 结果为 TRUE。也就是说，在进行比较运算的时候拿用户定义的比较函数来替代原有的比较运算符。

###### [lcof 51] 逆序数  https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/   🌟🌟🌟🌟

~~~cpp
int mergeSort(vector<int>& nums, vector<int>& tmp, int l, int r){
    if(l >= r) return 0;
    int mid = l + (r - l) / 2;
    // l -> mid 和 mid+1 -> r
    int inv_cnt = mergeSort(nums, tmp, l, mid) + mergeSort(nums, tmp, mid+1, r);

    int i = l, j = mid+1, pos = l;
    while(i <= mid && j <= r){
        if(nums[i] <= nums[j]){
            tmp[pos++] = nums[i++];
            inv_cnt += (j - mid - 1); // j - mid - 1
        }else{
            tmp[pos++] = nums[j++];
        }
    }

    while(i <= mid){
        tmp[pos++] = nums[i++];
        inv_cnt += (j - mid - 1);
    }
    while(j <= r) tmp[pos++] = nums[j++];
    copy(tmp.begin() + l, tmp.begin() + r + 1, nums.begin() + l);
    return inv_cnt;
}


int reversePairs(vector<int>& nums) {
    if(nums.size() == 0) return 0;
    vector<int> tmp(nums.size(), 0);
    return mergeSort(nums, tmp, 0, nums.size()-1);
}


~~~

###### [912] 排序数组 https://leetcode-cn.com/problems/sort-an-array/    🌟🌟🌟🌟🌟

~~~cpp
int partition(vector<int> & nums, int start, int end){
    int index = start + (end - start) / 2;   // 获取中间值的下标
    swap(nums[start], nums[index]);  // 交换
    int pivot = nums[start];  // 取轴

    int left = start, right = end;  // 定左右
    while(left != right){
        // 注意: 下面两行的顺序是不能换的!
        while(left < right && nums[right] >= pivot) right--;
        while(left < right && nums[left] <= pivot) left++;
        if(left < right) swap(nums[left], nums[right]);
    }
    swap(nums[start], nums[left]);  // 再次交换
    return left;   // ! 注意返回 left
}

void quickSort(vector<int>& nums, int start, int end){
    if(start < end){
        int index = partition(nums, start, end);
        quickSort(nums, start, index-1);
        quickSort(nums, index+1, end);
    }
}

vector<int> sortArray(vector<int>& nums){
    if(nums.size() == 0) return nums;
    quickSort(nums, 0, nums.size()-1);
    return nums;
}
~~~



### 5. 系统(多线程)与设计   

###### [面试题 16.25. LRU缓存] https://leetcode-cn.com/problems/lru-cache-lcci/  🌟🌟🌟🌟🌟🌟

~~~cpp
// 使用 哈希双向链表
// 注意三个数据结构不同的操作方法， 别弄混了就可以了。
// map:  添加/修改 map[key]   删除: erase(x)

// list:  删除: cache.erase()
//        back()    front()    pop_x()   push_x()  
//        获取迭代器: .begin()     .end()
//        通过迭代器获取值  *iter   

// pair:  获取值 kv.second  kv.first
public:
    LRUCache(int capacity) {
        max_capacity = capacity;
    }
    
    int get(int key) {
        if(m_map.find(key) != m_map.end()){
            auto kv = *m_map[key]; // 获取
          
            cache.erase(m_map[key]);  // 删除
            cache.push_front(kv);  // 添加
            m_map[key] = cache.begin(); // 更改指向
            return kv.second;
        }
        return -1;
    }
    
    void put(int key, int value) {
        if (m_map.find(key) == m_map.end()) {
            if (cache.size() == max_capacity) {
                m_map.erase(cache.back().first);  // 删除   !!! 需要删除 key 值
                cache.pop_back(); // 
            }
        }
        else {
            cache.erase(m_map[key]);
        }
        cache.push_front({key, value});
        m_map[key] = cache.begin();
    }
private:
    int max_capacity;
    list<pair<int, int>> cache;
    unordered_map<int, list<pair<int, int>>::iterator> m_map; // int-迭代
~~~
