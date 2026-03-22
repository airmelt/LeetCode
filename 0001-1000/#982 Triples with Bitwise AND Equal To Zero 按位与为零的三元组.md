# 982 Triples with Bitwise AND Equal To Zero 按位与为零的三元组

__Description__:
Given an integer array nums, return the number of AND triples.

An AND triple is a triple of indices (i, j, k) such that:

0 <= i < nums.length
0 <= j < nums.length
0 <= k < nums.length
nums[i] & nums[j] & nums[k] == 0, where & represents the bitwise-AND operator.

__Example:__

Example 1:

Input: nums = [2,1,3]
Output: 12
Explanation: We could choose the following i, j, k triples:
(i=0, j=0, k=1) : 2 & 2 & 1
(i=0, j=1, k=0) : 2 & 1 & 2
(i=0, j=1, k=1) : 2 & 1 & 1
(i=0, j=1, k=2) : 2 & 1 & 3
(i=0, j=2, k=1) : 2 & 3 & 1
(i=1, j=0, k=0) : 1 & 2 & 2
(i=1, j=0, k=1) : 1 & 2 & 1
(i=1, j=0, k=2) : 1 & 2 & 3
(i=1, j=1, k=0) : 1 & 1 & 2
(i=1, j=2, k=0) : 1 & 3 & 2
(i=2, j=0, k=1) : 3 & 2 & 1
(i=2, j=1, k=0) : 3 & 1 & 2

Example 2:

Input: nums = [0,0,0]
Output: 27

__Constraints:__

1 <= nums.length <= 1000
0 <= nums[i] < 2^16

__题目描述__:
给定一个整数数组 A，找出索引为 (i, j, k) 的三元组，使得：

0 <= i < A.length
0 <= j < A.length
0 <= k < A.length
A[i] & A[j] & A[k] == 0，其中 & 表示按位与（AND）操作符。

__示例 :__

输入：[2,1,3]
输出：12
解释：我们可以选出如下 i, j, k 三元组：
(i=0, j=0, k=1) : 2 & 2 & 1
(i=0, j=1, k=0) : 2 & 1 & 2
(i=0, j=1, k=1) : 2 & 1 & 1
(i=0, j=1, k=2) : 2 & 1 & 3
(i=0, j=2, k=1) : 2 & 3 & 1
(i=1, j=0, k=0) : 1 & 2 & 2
(i=1, j=0, k=1) : 1 & 2 & 1
(i=1, j=0, k=2) : 1 & 2 & 3
(i=1, j=1, k=0) : 1 & 1 & 2
(i=1, j=2, k=0) : 1 & 3 & 2
(i=2, j=0, k=1) : 3 & 2 & 1
(i=2, j=1, k=0) : 3 & 1 & 2

__提示:__

1 <= A.length <= 1000
0 <= A[i] < 2^16

__思路__:

状态压缩
用一个 count 数组记录两个数与的结果
然后查询能够和当前数字与的结果为 0 的 count 中的对应值
每次消除最后一位 x = (x - 1) & x
注意需要将本身是 0 的也加上
时间复杂度为 O(n ^ 2), 空间复杂度为 O(m), m 为 max(nums)

__代码__:
__C++__:

```C++
class Solution
{
public:
    int countTriplets(vector<int>& nums) 
    {
        int max_value = 1 << 16, n = nums.size(), result = 0;
        vector<int> count(max_value);
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) ++count[nums[i] & nums[j]];
        for (int i = 0; i < n; i++) for (int cur = max_value - 1 - nums[i], j = cur; j > 0; j = (j - 1) & cur) result += count[j];
        for (int i = 0; i < n; i++) result += count.front();
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countTriplets(int[] nums) {
        int maxValue = 1 << 16, n = nums.length, result = 0, count[] = new int[maxValue];
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) ++count[nums[i] & nums[j]];
        for (int i = 0; i < n; i++) for (int cur = maxValue - 1 - nums[i], j = cur; j > 0; j = (j - 1) & cur) result += count[j];
        for (int i = 0; i < n; i++) result += count[0];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countTriplets(self, nums: List[int]) -> int:
        result, n, max_value = 0, len(nums), 1 << 16
        count = Counter([nums[i] & nums[j] for i in range(n) for j in range(n)])
        for i in range(n):
            cur = j = max_value - 1 - nums[i]
            result += count[0]
            while j > 0:
                result += count[j]
                j = (j - 1) & cur
        return result
```
