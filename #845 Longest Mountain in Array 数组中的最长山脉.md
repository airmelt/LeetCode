# 845 Longest Mountain in Array 数组中的最长山脉

__Description__:
You may recall that an array arr is a mountain array if and only if:

arr.length >= 3
There exists some index i (0-indexed) with 0 < i < arr.length - 1 such that:
arr[0] < arr[1] < ... < arr[i - 1] < arr[i]
arr[i] > arr[i + 1] > ... > arr[arr.length - 1]
Given an integer array arr, return the length of the longest subarray, which is a mountain. Return 0 if there is no mountain subarray.

__Example:__

Example 1:

Input: arr = [2,1,4,7,3,2,5]
Output: 5
Explanation: The largest mountain is [1,4,7,3,2] which has length 5.

Example 2:

Input: arr = [2,2,2]
Output: 0
Explanation: There is no mountain.

__Constraints:__

1 <= arr.length <= 10^4
0 <= arr[i] <= 10^4

__Follow up:__

Can you solve it using only one pass?
Can you solve it in O(1) space?

__题目描述__:
我们把数组 A 中符合下列属性的任意连续子数组 B 称为 “山脉”：

B.length >= 3
存在 0 < i < B.length - 1 使得 B[0] < B[1] < ... B[i-1] < B[i] > B[i+1] > ... > B[B.length - 1]
（注意：B 可以是 A 的任意子数组，包括整个数组 A。）

给出一个整数数组 A，返回最长 “山脉” 的长度。

如果不含有 “山脉” 则返回 0。

__示例 :__

示例 1：

输入：[2,1,4,7,3,2,5]
输出：5
解释：最长的 “山脉” 是 [1,4,7,3,2]，长度为 5。

示例 2：

输入：[2,2,2]
输出：0
解释：不含 “山脉”。

__提示:__

0 <= A.length <= 10000
0 <= A[i] <= 10000

__思路__:

滑动窗口
固定左侧山脚 left, left + 2 < n, 山脉长度至少为 3, 且 arr[left] < arr[left + 1], 否则查找下一个元素
右侧山脚从 left + 1 开始查找, 找到第一个不再递增的元素, 这时为山顶, 判断是否为山顶再找到第一个不递减的元素, 就为右侧山脚, 更新结果长度
将 right 赋给 left 继续查找下一个山脉
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int longestMountain(vector<int>& arr) 
    {
        int n = arr.size(), result = 0, left = 0;
        while (left + 2 < n) 
        {
            int right = left + 1;
            if (arr[left] < arr[left + 1]) 
            {
                while (right + 1 < n and arr[right] < arr[right + 1]) ++right;
                if (right < n - 1 and arr[right] > arr[right + 1]) 
                {
                    while (right + 1 < n and arr[right] > arr[right + 1]) ++right;
                    result = max(result, right - left + 1);
                } 
                else ++right;
            }
            left = right;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestMountain(int[] arr) {
        int n = arr.length, result = 0, left = 0;
        while (left + 2 < n) {
            int right = left + 1;
            if (arr[left] < arr[left + 1]) {
                while (right + 1 < n && arr[right] < arr[right + 1]) ++right;
                if (right < n - 1 && arr[right] > arr[right + 1]) {
                    while (right + 1 < n && arr[right] > arr[right + 1]) ++right;
                    result = Math.max(result, right - left + 1);
                } else ++right;
            }
            left = right;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def longestMountain(self, arr: List[int]) -> int:
        return max(map(lambda m: 1 + len(m.group()), re.finditer(r'\++\-+', ''.join('+' if arr[i] > arr[i - 1] else ('0' if arr[i] == arr[i-1] else '-') for i in range(1, len(arr))))), default=0)
```
