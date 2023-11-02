# 1865 Finding Pairs With a Certain Sum 找出和为指定值的下标对

__Description:__

You are given two integer arrays `nums1` and `nums2`. You are tasked to implement a data structure that supports queries of two types:

Implement the `FindSumPairs` class:

- `FindSumPairs(int[] nums1, int[] nums2)` Initializes the `FindSumPairs` object with two integer arrays `nums1` and `nums2`.
- `void add(int index, int val)` Adds `val` to `nums2[index]`, i.e., apply `nums2[index] += val`.
- `int count(int tot)` Returns the number of pairs `(i, j)` such that `nums1[i] + nums2[j] == tot`.

__Example:__

Example 1:

```text
Input
["FindSumPairs", "count", "add", "count", "count", "add", "add", "count"]
[[[1, 1, 2, 2, 2, 3], [1, 4, 5, 2, 5, 4]], [7], [3, 2], [8], [4], [0, 1], [1, 1], [7]]
Output
[null, 8, null, 2, 1, null, null, 11]

Explanation
FindSumPairs findSumPairs = new FindSumPairs([1, 1, 2, 2, 2, 3], [1, 4, 5, 2, 5, 4]);
findSumPairs.count(7); // return 8; pairs (2,2), (3,2), (4,2), (2,4), (3,4), (4,4) make 2 + 5 and pairs (5,1), (5,5) make 3 + 4
findSumPairs.add(3, 2); // now nums2 = [1,4,5,4`,5,4`]
findSumPairs.count(8); // return 2; pairs (5,2), (5,4) make 3 + 5
findSumPairs.count(4); // return 1; pair (5,0) makes 3 + 1
findSumPairs.add(0, 1); // now nums2 = [`2`,4,5,4 `,5,4`]
findSumPairs.add(1, 1); // now nums2 = [ `2`,5,5,4 `,5,4`]
findSumPairs.count(7); // return 11; pairs (2,1), (2,2), (2,4), (3,1), (3,2), (3,4), (4,1), (4,2), (4,4) make 2 + 5 and pairs (5,3), (5,5) make 3 + 4
```

__Constraints:__

- `1 <= nums1.length <= 1000`
- `1 <= nums2.length <= 10 ^ 5`
- `1 <= nums1[i] <= 10 ^ 9`
- `1 <= nums2[i] <= 10 ^ 5`
- `0 <= index < nums2.length`
- `1 <= val <= 10 ^ 5`
- `1 <= tot <= 10 ^ 9`
- At most `1000` calls are made to `add` and `count` __each__.

__题目描述:__

给你两个整数数组 `nums1` 和 `nums2` ，请你实现一个支持下述两类查询的数据结构:

实现 `FindSumPairs` 类:

- `FindSumPairs(int[] nums1, int[] nums2)` 使用整数数组 `nums1` 和 `nums2` 初始化 `FindSumPairs` 对象。
- `void add(int index, int val)` 将 `val` 加到 `nums2[index]` 上，即，执行 `nums2[index] += val` 。
- `int count(int tot)` 返回满足 `nums1[i] + nums2[j] == tot` 的下标对 `(i, j)` 数目。

__示例:__

示例：

```text
输入: 
["FindSumPairs", "count", "add", "count", "count", "add", "add", "count"]
[[[1, 1, 2, 2, 2, 3], [1, 4, 5, 2, 5, 4]], [7], [3, 2], [8], [4], [0, 1], [1, 1], [7]]
输出: 
[null, 8, null, 2, 1, null, null, 11]

解释: 
FindSumPairs findSumPairs = new FindSumPairs([1, 1, 2, 2, 2, 3], [1, 4, 5, 2, 5, 4]);
findSumPairs.count(7); // 返回 8 ; 下标对 (2,2), (3,2), (4,2), (2,4), (3,4), (4,4) 满足 2 + 5 = 7 ，下标对 (5,1), (5,5) 满足 3 + 4 = 7
findSumPairs.add(3, 2); // 此时 nums2 = [1,4,5,_4_`,5,4`]
findSumPairs.count(8); // 返回 2 ；下标对 (5,2), (5,4) 满足 3 + 5 = 8
findSumPairs.count(4); // 返回 1 ；下标对 (5,0) 满足 3 + 1 = 4
findSumPairs.add(0, 1); // 此时 nums2 = [_`2`_,4,5,4 `,5,4`]
findSumPairs.add(1, 1); // 此时 nums2 = [ `2`,_5_,5,4 `,5,4`]
findSumPairs.count(7); // 返回 11 ；下标对 (2,1), (2,2), (2,4), (3,1), (3,2), (3,4), (4,1), (4,2), (4,4) 满足 2 + 5 = 7 ，下标对 (5,3), (5,5) 满足 3 + 4 = 7
```

__提示：__

- `1 <= nums1.length <= 1000`
- `1 <= nums2.length <= 10 ^ 5`
- `1 <= nums1[i] <= 10 ^ 9`
- `1 <= nums2[i] <= 10 ^ 5`
- `0 <= index < nums2.length`
- `1 <= val <= 10 ^ 5`
- `1 <= tot <= 10 ^ 9`
- 最多调用 `add` 和 `count` 函数各 `1000` 次

__思路:__

```text
哈希表
用哈希表存储 nums2 的值的个数
每次更新时, 先减少对应 nums2[index] 的哈希值
更新值之后再加上对应 nums2[index] 的值
计算对数时, 只需要查找差值在哈希表中的和
初始化时间复杂度为 O(M + N), add() 时间复杂度为 O(1), count() 时间复杂度为 O(N), 空间复杂度为 O(M + N), N 和 M 分别为 nums1 和 nums2 的长度
```

__代码:__

__C++__:

```C++
class FindSumPairs 
{
public:
    FindSumPairs(vector<int>& nums1, vector<int>& nums2) 
    {
        this -> nums1 = nums1;
        this -> nums2 = nums2;
        for (const auto& num: nums2) ++d2[num];
    }
    
    void add(int index, int val) 
    {
        --d2[nums2[index]];
        nums2[index] += val;
        ++d2[nums2[index]];
    }
    
    int count(int tot) 
    {
        int result = 0, rest;
        for (const auto& num : nums1) if (d2.count(rest = tot - num)) result += d2[rest];
        return result;
    }
private:
    vector<int> nums1, nums2;
    unordered_map<int, int> d2;
};

/**
 * Your FindSumPairs object will be instantiated and called as such:
 * FindSumPairs* obj = new FindSumPairs(nums1, nums2);
 * obj->add(index,val);
 * int param_2 = obj->count(tot);
 */
```

__Java__:

```Java
class FindSumPairs {
    private int[] nums1, nums2;
    private Map<Integer, Integer> map = new HashMap<>();

    public FindSumPairs(int[] nums1, int[] nums2) {
        this.nums1 = nums1;
        this.nums2 = nums2;
        for (int num : nums2) map.merge(num, 1, Integer::sum);
    }
    
    public void add(int index, int val) {
        map.merge(nums2[index], -1, Integer::sum);
        nums2[index] += val;
        map.merge(nums2[index], 1, Integer::sum);
    }
    
    public int count(int tot) {
        int result = 0, rest = 0;
        for (int num : nums1) if (map.containsKey(rest = tot - num)) result += map.get(rest);
        return result;
    }
}

/**
 * Your FindSumPairs object will be instantiated and called as such:
 * FindSumPairs obj = new FindSumPairs(nums1, nums2);
 * obj.add(index,val);
 * int param_2 = obj.count(tot);
 */
```

__Python__:

```Python
class FindSumPairs:

    def __init__(self, nums1: List[int], nums2: List[int]):
        self.nums1 = nums1
        self.nums2 = nums2
        self.d2 = Counter(nums2)


    def add(self, index: int, val: int) -> None:
        self.d2[self.nums2[index]] -= 1
        self.nums2[index] += val
        self.d2[self.nums2[index]] += 1


    def count(self, tot: int) -> int:
        result = 0
        for num in self.nums1:
            if (rest := tot - num) in self.d2:
                result += self.d2[rest]
        return result


# Your FindSumPairs object will be instantiated and called as such:
# obj = FindSumPairs(nums1, nums2)
# obj.add(index,val)
# param_2 = obj.count(tot)
```
