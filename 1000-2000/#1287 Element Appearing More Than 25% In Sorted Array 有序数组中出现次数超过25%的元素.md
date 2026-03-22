# 1287 Element Appearing More Than 25% In Sorted Array 有序数组中出现次数超过25%的元素

__Description__:
Given an integer array sorted in non-decreasing order, there is exactly one integer in the array that occurs more than 25% of the time.

Return that integer.

__Example:__

Example 1:

Input: arr = [1,2,2,6,6,6,6,7,10]
Output: 6

__Constraints:__

1 <= arr.length <= 10^4
0 <= arr[i] <= 10^5

__题目描述__:
给你一个非递减的 有序 整数数组，已知这个数组中恰好有一个整数，它的出现次数超过数组元素总数的 25%。

请你找到并返回这个整数

__示例 :__

输入：arr = [1,2,2,6,6,6,6,7,10]
输出：6

__提示：__

1 <= arr.length <= 10^4
0 <= arr[i] <= 10^5

__思路__:

由于数组有序, 如果这个数是出现次数大于 25%的, 那么这个数和后面的 arr.size() / 4个数应该都是相等的, 按这个步长查找即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findSpecialInteger(vector<int>& arr) 
    {
        for (int i = 0, step = arr.size() / 4; i < arr.size() - step; i++) if (arr[i] == arr[i + step]) return arr[i];
        return arr[0];
    }
};
```

__Java__:

```Java
class Solution {
    public int findSpecialInteger(int[] arr) {
        for (int i = 0, step = arr.length / 4; i < arr.length - step; i++) if (arr[i] == arr[i + step]) return arr[i];
        return arr[0];
    }
}
```

__Python__:

```Python
class Solution:
    def findSpecialInteger(self, arr: List[int]) -> int:
        return collections.Counter(arr).most_common(1)[0][0]
```
