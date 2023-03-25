# 1574 Shortest Subarray to be Removed to Make Array Sorted 删除最短的子数组使剩余数组有序

__Description:__

Given an integer array `arr`, remove a subarray (can be empty) from `arr` such that the remaining elements in `arr` are __non-decreasing__.

Return the length of the shortest subarray to remove.

A subarray is a contiguous subsequence of the array.

__Example:__

Example 1:

```text
Input: arr = [1,2,3,10,4,2,3,5]
Output: 3
Explanation: The shortest subarray we can remove is [10,4,2] of length 3. The remaining elements after that will be [1,2,3,3,5] which are sorted.
Another correct solution is to remove the subarray [3,10,4].
```

Example 2:

```text
Input: arr = [5,4,3,2,1]
Output: 4
Explanation: Since the array is strictly decreasing, we can only keep a single element. Therefore we need to remove a subarray of length 4, either [5,4,3,2] or [4,3,2,1].
```

Example 3:

```text
Input: arr = [1,2,3]
Output: 0
Explanation: The array is already non-decreasing. We do not need to remove any elements.
```

__Constraints:__

- `1 <= arr.length <= 10 ^ 5`
- `0 <= arr[i] <= 10 ^ 9`

__题目描述:__

给你一个整数数组 `arr` ，请你删除一个子数组（可以为空），使得 `arr` 中剩下的元素是 __非递减__ 的。

一个子数组指的是原数组中连续的一个子序列。

请你返回满足题目要求的最短子数组的长度。

__示例:__

示例 1：

```text
输入：arr = [1,2,3,10,4,2,3,5]
输出：3
解释：我们需要删除的最短子数组是 [10,4,2] ，长度为 3 。剩余元素形成非递减数组 [1,2,3,3,5] 。
另一个正确的解为删除子数组 [3,10,4] 。
```

示例 2：

```text
输入：arr = [5,4,3,2,1]
输出：4
解释：由于数组是严格递减的，我们只能保留一个元素。所以我们需要删除长度为 4 的子数组，要么删除 [5,4,3,2]，要么删除 [4,3,2,1]。
```

示例 3：

```text
输入：arr = [1,2,3]
输出：0
解释：数组已经是非递减的了，我们不需要删除任何元素。
```

示例 4：

```text
输入：arr = [1]
输出：0
```

__提示：__

- `1 <= arr.length <= 10 ^ 5`
- `0 <= arr[i] <= 10 ^ 9`

__思路:__

```text
双指针
先检查数组是否已经有序
枚举 i, j, 使得 
arr[0] ~ arr[i] 非递减
arr[j] ~ arr[n - 1] 非递减
arr[i] <= arr[j]
只需要找到右边界使得 arr[j] ~ arr[n - 1] 非递减, 那么每次 j 一定移动的部分都是非递减的
每次更新右边界, 那么 j - i - 1 的部分都应该被移除
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findLengthOfShortestSubarray(vector<int>& arr) 
    {
        int n = arr.size(), j = n - 1;
        while (j and arr[j - 1] <= arr[j]) --j;
        int result = j;
        if (!j) return result;
        for (int i = 0; i < n; i++) 
        {
            while (j < n and arr[j] < arr[i]) ++j;
            result = min(result, j - i - 1);
            if (i + 1 < n and arr[i] > arr[i + 1]) break;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findLengthOfShortestSubarray(int[] arr) {
        int n = arr.length, j = n - 1;
        while (j > 0 && arr[j - 1] <= arr[j]) --j;
        int result = j;
        if (j == 0) return result;
        for (int i = 0; i < n; i++) {
            while (j < n && arr[j] < arr[i]) ++j;
            result = Math.min(result, j - i - 1);
            if (i + 1 < n && arr[i] > arr[i + 1]) break;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findLengthOfShortestSubarray(self, arr: List[int]) -> int:
        j = (n := len(arr)) - 1
        while j > 0 and arr[j - 1] <= arr[j]:
            j -= 1
        result = j
        if not j:
            return result
        for i in range(n):
            while j < n and arr[j] < arr[i]:
                j += 1
            result = min(result, j - i - 1)
            if i + 1 < n and arr[i] > arr[i + 1]:
                break
        return result
```
