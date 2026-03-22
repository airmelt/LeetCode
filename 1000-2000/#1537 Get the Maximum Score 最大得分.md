# 1537 Get the Maximum Score 最大得分

__Description:__

You are given two __sorted__ arrays of distinct integers `nums1` and `nums2.`

A valid path is defined as follows:

- Choose array `nums1` or `nums2` to traverse (from index-0).
- Traverse the current array from left to right.
- If you are reading any value that is present in `nums1` and `nums2` you are allowed to change your path to the other array. (Only one repeated value is considered in the valid path).

The score is defined as the sum of uniques values in a valid path.

Return _the maximum score you can obtain of all possible __valid paths___. Since the answer may be too large, return it modulo `10 ^ 9 + 7`.

__Example:__

Example 1:

![1537-1](https://assets.leetcode.com/uploads/2020/07/16/sample_1_1893.png)

```text
Input: nums1 = [2,4,5,8,10], nums2 = [4,6,8,9]
Output: 30
Explanation: Valid paths:
[2,4,5,8,10], [2,4,5,8,9], [2,4,6,8,9], [2,4,6,8,10],  (starting from nums1)
[4,6,8,9], [4,5,8,10], [4,5,8,9], [4,6,8,10]    (starting from nums2)
The maximum is obtained with the path in green [2,4,6,8,10].
```

Example 2:

```text
Input: nums1 = [1,3,5,7,9], nums2 = [3,5,100]
Output: 109
Explanation: Maximum sum is obtained with the path [1,3,5,100].
```

Example 3:

```text
Input: nums1 = [1,2,3,4,5], nums2 = [6,7,8,9,10]
Output: 40
Explanation: There are no common elements between nums1 and nums2.
Maximum sum is obtained with the path [6,7,8,9,10].
```

__Constraints:__

- `1 <= nums1.length, nums2.length <= 10 ^ 5`
- `1 <= nums1[i], nums2[i] <= 10 ^ 7`
- `nums1` and `nums2` are strictly increasing.

__题目描述:__

你有两个 __有序__ 且数组内元素互不相同的数组 `nums1` 和 `nums2` 。

一条 合法路径 定义如下：

- 选择数组 nums1 或者 nums2 开始遍历（从下标 0 处开始）。
- 从左到右遍历当前数组。
- 如果你遇到了 `nums1` 和 `nums2` 中都存在的值，那么你可以切换路径到另一个数组对应数字处继续遍历（但在合法路径中重复数字只会被统计一次）。

得分定义为合法路径中不同数字的和。

请你返回所有可能合法路径中的最大得分。

由于答案可能很大，请你将它对 10 ^ 9 + 7 取余后返回。

__示例:__

示例 1：

![1537-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/02/sample_1_1893.png)

```text
输入：nums1 = [2,4,5,8,10], nums2 = [4,6,8,9]
输出：30
解释：合法路径包括：
[2,4,5,8,10], [2,4,5,8,9], [2,4,6,8,9], [2,4,6,8,10],（从 nums1 开始遍历）
[4,6,8,9], [4,5,8,10], [4,5,8,9], [4,6,8,10]  （从 nums2 开始遍历）
最大得分为上图中的绿色路径 [2,4,6,8,10] 。
```

示例 2：

```text
输入：nums1 = [1,3,5,7,9], nums2 = [3,5,100]
输出：109
解释：最大得分由路径 [1,3,5,100] 得到。
```

示例 3：

```text
输入：nums1 = [1,2,3,4,5], nums2 = [6,7,8,9,10]
输出：40
解释：nums1 和 nums2 之间无相同数字。
最大得分由路径 [6,7,8,9,10] 得到。
```

示例 4：

```text
输入：nums1 = [1,4,5,8,9,11,19], nums2 = [2,3,4,11,12]
输出：61
```

__提示：__

- `1 <= nums1.length <= 10 ^ 5`
- `1 <= nums2.length <= 10 ^ 5`
- `1 <= nums1[i], nums2[i] <= 10 ^ 7`
- `nums1` 和 `nums2` 都是严格递增的数组。

__思路:__

```text
贪心
设置两个指针分别指向两个数组的开头
由于数组已经有序
每次让较小的指针向后移动, 并累积和
遇到交点的时候将结果加上累积的较大值, 并重置累积的和
最后还有没有遍历完的元素需要计入
注意在中间不能进行取余运算, 因为会使得比较大小不准确
时间复杂度为 O(M + N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxSum(vector<int>& nums1, vector<int>& nums2) 
    {
        int i = 0, j = 0, n1 = nums1.size(), n2 = nums2.size(), MOD = 1e9 + 7;
        long s1 = 0, s2 = 0, result = 0;
        while (i < n1 and j < n2) 
        {
            if (nums1[i] < nums2[j]) s1 += nums1[i++];
            else if (nums1[i] > nums2[j]) s2 += nums2[j++];
            else 
            {
                result += max(s1, s2) + nums1[i++];
                s1 = s2 = 0;
                ++j;
            }
        }
        while (i < n1) s1 += nums1[i++];
        while (j < n2) s2 += nums2[j++];
        return (result + max(s1, s2)) % MOD;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxSum(int[] nums1, int[] nums2) {
        int i = 0, j = 0, n1 = nums1.length, n2 = nums2.length, MOD = 1_000_000_007;
        long s1 = 0, s2 = 0, result = 0;
        while (i < n1 && j < n2) {
            if (nums1[i] < nums2[j]) s1 += nums1[i++];
            else if (nums1[i] > nums2[j]) s2 += nums2[j++];
            else {
                result += Math.max(s1, s2) + nums1[i++];
                s1 = s2 = 0;
                ++j;
            }
        }
        while (i < n1) s1 += nums1[i++];
        while (j < n2) s2 += nums2[j++];
        return (int)((result + Math.max(s1, s2)) % MOD);
    }
}
```

__Python__:

```Python
class Solution:
    def maxSum(self, nums1: List[int], nums2: List[int]) -> int:
        i, j, s1, s2, result, mod, n1, n2 = 0, 0, 0, 0, 0, 10 ** 9 + 7, len(nums1), len(nums2)
        while i < n1 and j < n2:
            if nums1[i] < nums2[j]:
                s1 += nums1[i]
                i += 1
            elif nums1[i] > nums2[j]:
                s2 += nums2[j]
                j += 1
            else:
                result += max(s1, s2) + nums1[i]
                s1, s2 = 0, 0
                i += 1
                j += 1
        s1 += sum(nums1[i:])
        s2 += sum(nums2[j:])
        return (result + max(s1, s2)) % mod
```
