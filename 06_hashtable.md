
## 字典 map 的使用  

- STL 提供了  Map/MultiMap 和 Unordered Map/Multi-Map 来进行字典操作， 其中前者的底层是红黑树，后者的底层才是 哈希表

- 进行索引时， 如果没有 key， 则会自动插入 key， 且如果 value 为 int， 则初始化为 0

  ~~~cpp
  for k in keys:
      m_map[k]++;
  ~~~

- 经常会使用数组来代替hashmap， 形成 index -> value 这样一种映射， 这种操作在字符串中尤为常见。 

- 常见的 set/map API

  ~~~cpp
  begin()   end()   empty()   size()
  erase(iterator)       erase(key_value)
  find()
  s.insert(key_value); // set  
  m[key] = val;  // map
  ~~~

###### [347] 前 K 个高频元素 https://leetcode-cn.com/problems/top-k-frequent-elements/

~~~cpp
vector<int> topKFrequent(vector<int>& nums, int k) {
    vector<int> res;

    map<int, int> m_map;
    for(auto n:nums)  m_map[n]++; // 如果 key 不存在，则自动添加， 且 value 初始化为 0

    // 将 map 转化为 vector 进行排序
    vector<pair<int, int> > pair_vec(m_map.begin(), m_map.end()); 
    sort(pair_vec.begin(), pair_vec.end(), 
            [](pair<int, int> a, pair<int, int> b){return a.second > b.second;});  // lambda 算子

    int i = 0;
    for(auto p: pair_vec){
        if(i++ < k) res.push_back(p.first); 
    }
    return res;
}
~~~

###### [387] 字符串中的第一个唯一字符 https://leetcode-cn.com/problems/first-unique-character-in-a-string/

~~~cpp
// 不要使用 map, 直接使用 数组 作为 哈希表即可
char firstUniqChar(string s) {
    vector<int> dic(26, 0);
    for(auto c:s)   dic[ c - 'a'] += 1;
    for(auto c:s)   if(dic[c - 'a'] == 1) return c;
    return ' '; // 可以直接 return  ' ' 的
}
~~~

###### [lcof 57] 和为s的两个数字 https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/

~~~cpp
// 要求返回下标时 -> 使用哈希表解决
//    返回数值时 -> 使用排序 + 双指针
vector<int> twoSum(vector<int>& nums, int target) {
    set<int> m_set;
    vector<int> res;

    for(auto n : nums){
        if(m_set.find(target - n) == m_set.end()){
            m_set.insert(n);
        }else{
            res.push_back(n);
            res.push_back(target - n);
            break;
        }
    }
    return res; 
}
~~~

###### [242]. 有效的字母异位词 https://leetcode-cn.com/problems/valid-anagram/

~~~cpp
bool isAnagram(string s, string t) {
    vector<char> m_vec(26, 0);

    for(auto c:s) m_vec[c-'a']++;
    for(auto c:t) m_vec[c-'a']--;
    for(int i=0; i < m_vec.size(); i++)
        if(m_vec[i] != 0) return false;
    return true;
}
~~~

###### [49] 字母异位词分组 https://leetcode-cn.com/problems/group-anagrams/

~~~cpp
vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<vector<string> > res;
        map<string, vector<string> >  m_map;

        for(auto str: strs){
            string ori_str = str;
            sort(str.begin(), str.end());
            m_map[str].push_back(ori_str);
        }

        for(auto it:m_map) res.push_back(it.second);
        return res;
}
~~~

###### [448] 找到所有数组中消失的数字 https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/

~~~cpp
vector<int> findDisappearedNumbers(vector<int>& nums) {
    vector<int> res;

    for(int i = 0; i < nums.size(); i++){
        int index = abs(nums[i]) - 1;
        if(nums[index] > 0) 
            nums[index] = -1 * nums[index];
    }

    for(int i = 0;  i < nums.size(); i++){
        if(nums[i] > 0) res.push_back(i+1);
    }
    return res;
}
~~~
