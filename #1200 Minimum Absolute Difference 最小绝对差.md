# 1200 Minimum Absolute Difference 最小绝对差

__Description__:
Given an array of distinct integers arr, find all pairs of elements with the minimum absolute difference of any two elements.

Return a list of pairs in ascending order(with respect to pairs), each pair [a, b] follows

a, b are from arr
a < b
b - a equals to the minimum absolute difference of any two elements in arr

__Example:__

Example 1:

Input: arr = [4,2,1,3]
Output: [[1,2],[2,3],[3,4]]
Explanation: The minimum absolute difference is 1. List all pairs with difference equal to 1 in ascending order.

Example 2:

Input: arr = [1,3,6,10,15]
Output: [[1,3]]

Example 3:

Input: arr = [3,8,-10,23,19,-4,-14,27]
Output: [[-14,-10],[19,23],[23,27]]

__Constraints:__

2 <= arr.length <= 10^5
-10^6 <= arr[i] <= 10^6

__题目描述__:
给你个整数数组 arr，其中每个元素都 不相同。

请你找到所有具有最小绝对差的元素对，并且按升序的顺序返回。

__示例 :__

示例 1：

输入：arr = [4,2,1,3]
输出：[[1,2],[2,3],[3,4]]

示例 2：

输入：arr = [1,3,6,10,15]
输出：[[1,3]]

示例 3：

输入：arr = [3,8,-10,23,19,-4,-14,27]
输出：[[-14,-10],[19,23],[23,27]]

__提示：__

2 <= arr.length <= 10^5
-10^6 <= arr[i] <= 10^6

__思路__:

先排序, 遍历两次数组, 第一次找相邻的两个数的最小差值, 即为最小绝对差, 然后找到等于最小绝对差的两个数插入结果
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> minimumAbsDifference(vector<int>& arr) 
    {
        sort(arr.begin(), arr.end());
        int min_value = INT_MAX;
        for (int i = 1; i < arr.size(); ++i) min_value = min(min_value, arr[i] - arr[i - 1]);
        vector<vector<int>> result;
        for (int i = 1; i < arr.size(); ++i) if (arr[i] - arr[i - 1] == min_value) result.push_back({arr[i - 1], arr[i]});
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> minimumAbsDifference(int[] arr) {
        Arrays.sort(arr);
        int minValue = Integer.MAX_VALUE;
        for (int i = 1; i < arr.length; ++i) minValue = Math.min(minValue, arr[i] - arr[i - 1]);
        List<List<Integer>> result = new ArrayList<>();
        for (int i = 1; i < arr.length; ++i) if (arr[i] - arr[i - 1] == minValue) result.add(Arrays.asList(arr[i - 1], arr[i]));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumAbsDifference(self, arr: List[int]) -> List[List[int]]:
        arr = sorted(arr)
        l = [arr[i + 1] - arr[i] for i in range(len(arr) - 1)]
        a = min(l)
        return [[arr[i], arr[i + 1]] for i in range(len(l)) if arr[i + 1] - arr[i] == a]
```
