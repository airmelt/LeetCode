# 2080 Range Frequency Queries 区间内查询数字的频率

__Description:__

Design a data structure to find the frequency of a given value in a given subarray.

The frequency of a value in a subarray is the number of occurrences of that value in the subarray.

Implement the `RangeFreqQuery` class:

- `RangeFreqQuery(int[] arr)` Constructs an instance of the class with the given __0-indexed__ integer array `arr`.
- `int query(int left, int right, int value)` Returns the __frequency__ of `value` in the subarray `arr[left...right]`.

A __subarray__ is a contiguous sequence of elements within an array. `arr[left...right]` denotes the subarray that contains the elements of `nums` between indices `left` and `right` (__inclusive__).

__Example:__

Example 1:

```text
Input
["RangeFreqQuery", "query", "query"]
[[[12, 33, 4, 56, 22, 2, 34, 33, 22, 12, 34, 56]], [1, 2, 4], [0, 11, 33]]
Output
[null, 1, 2]

Explanation
```

```Java
RangeFreqQuery rangeFreqQuery = new RangeFreqQuery([12, 33, 4, 56, 22, 2, 34, 33, 22, 12, 34, 56]);
rangeFreqQuery.query(1, 2, 4); // return 1. The value 4 occurs 1 time in the subarray [33, 4]
rangeFreqQuery.query(0, 11, 33); // return 2. The value 33 occurs 2 times in the whole array.
```

__Constraints:__

- `1 <= arr.length <= 10 ^ 5`
- `1 <= arr[i], value <= 10 ^ 4`
- `0 <= left <= right < arr.length`
- At most `10 ^ 5` calls will be made to `query`

__题目描述:__

请你设计一个数据结构，它能求出给定子数组内一个给定值的 频率 。

子数组中一个值的 频率 指的是这个子数组中这个值的出现次数。

请你实现 `RangeFreqQuery` 类:

- `RangeFreqQuery(int[] arr)` 用下标从 __0__ 开始的整数数组 `arr` 构造一个类的实例。
- `int query(int left, int right, int value)` 返回子数组 `arr[left...right]` 中 `value` 的 __频率__ 。

一个 __子数组__ 指的是数组中一段连续的元素。 `arr[left...right]` 指的是 `nums` 中包含下标 `left` 和 `right` __在内__ 的中间一段连续元素。

__示例:__

示例 1：

```text
输入：
["RangeFreqQuery", "query", "query"]
[[[12, 33, 4, 56, 22, 2, 34, 33, 22, 12, 34, 56]], [1, 2, 4], [0, 11, 33]]
输出：
[null, 1, 2]

解释：
```

```Java
RangeFreqQuery rangeFreqQuery = new RangeFreqQuery([12, 33, 4, 56, 22, 2, 34, 33, 22, 12, 34, 56]);
rangeFreqQuery.query(1, 2, 4); // 返回 1 。4 在子数组 [33, 4] 中出现 1 次。
rangeFreqQuery.query(0, 11, 33); // 返回 2 。33 在整个子数组中出现 2 次。
```

__提示：__

- `1 <= arr.length <= 10 ^ 5`
- `1 <= arr[i], value <= 10 ^ 4`
- `0 <= left <= right < arr.length`
- 调用 `query` 不超过 `10 ^ 5` 次。

__思路:__

```text
哈希表 ➕ 二分
用哈希表记录每一个数值出现的位置
则位置本身是有序的
对于每一个查询, 二分查找出现的位置
查询最后一个大于等于 right + 1 的位置和第一个位置大于等于 left 的位置, 相减即可得到频率
即 upper_bound/biscet_right, right 和 lower_bound/bisect_left, left
时间复杂度为 O(N + QlogN), 空间复杂度为 O(N), 其中 N 为 arr 的长度, Q 为查询的次数
```

__代码:__

__C++__:

```C++
class RangeFreqQuery 
{
public:
    RangeFreqQuery(vector<int>& arr) 
    {
        for (int i = 0, n = arr.size(); i < n; i++) data[arr[i]].emplace_back(i);
    }
    
    int query(int left, int right, int value) 
    {
        return upper_bound(data[value].begin(), data[value].end(), right) - lower_bound(data[value].begin(), data[value].end(), left);
    }
private:
    unordered_map<int, vector<int>> data;
};

/**
 * Your RangeFreqQuery object will be instantiated and called as such:
 * RangeFreqQuery* obj = new RangeFreqQuery(arr);
 * int param_1 = obj->query(left,right,value);
 */
```

__Java__:

```Java
class RangeFreqQuery {
    private Map<Integer, List<Integer>> data = new HashMap<>();

    public RangeFreqQuery(int[] arr) {
        for (int i = 0, n = arr.length; i < n; i++) data.computeIfAbsent(arr[i], x -> new ArrayList<>()).add(i);
    }
    
    public int query(int left, int right, int value) {
        return find(data.get(value), right + 1) - find(data.get(value), left);
    }

    private int find(List<Integer> list, int target) {
        if (list == null) return 0;
        int left = 0, right = list.size();
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (list.get(mid) < target) left = mid + 1;
            else right = mid;
        }
        return left;
    }
}

/**
 * Your RangeFreqQuery object will be instantiated and called as such:
 * RangeFreqQuery obj = new RangeFreqQuery(arr);
 * int param_1 = obj.query(left,right,value);
 */
```

__Python__:

```Python
class RangeFreqQuery:

    def __init__(self, arr: List[int]):
        self.data = defaultdict(list)
        for i in range(len(arr)):
            self.data[arr[i]].append(i)


    def query(self, left: int, right: int, value: int) -> int:
        return bisect_right(self.data[value], right) - bisect_left(self.data[value], left)



# Your RangeFreqQuery object will be instantiated and called as such:
# obj = RangeFreqQuery(arr)
# param_1 = obj.query(left,right,value)
```
