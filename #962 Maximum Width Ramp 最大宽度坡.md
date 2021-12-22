# 962 Maximum Width Ramp 最大宽度坡

__Description__:
A ramp in an integer array nums is a pair (i, j) for which i < j and nums[i] <= nums[j]. The width of such a ramp is j - i.

Given an integer array nums, return the maximum width of a ramp in nums. If there is no ramp in nums, return 0.

__Example:__

Example 1:

Input: nums = [6,0,8,2,1,5]
Output: 4
Explanation: The maximum width ramp is achieved at (i, j) = (1, 5): nums[1] = 0 and nums[5] = 5.

Example 2:

Input: nums = [9,8,1,0,1,9,4,0,4,1]
Output: 7
Explanation: The maximum width ramp is achieved at (i, j) = (2, 9): nums[2] = 1 and nums[9] = 1.

__Constraints:__

2 <= nums.length <= 5 \* 10^4
0 <= nums[i] <= 5 \* 10^4

__题目描述__:
给定一个整数数组 A，坡是元组 (i, j)，其中  i < j 且 A[i] <= A[j]。这样的坡的宽度为 j - i。

找出 A 中的坡的最大宽度，如果不存在，返回 0 。

__示例 :__

示例 1：

输入：[6,0,8,2,1,5]
输出：4
解释：
最大宽度的坡为 (i, j) = (1, 5): A[1] = 0 且 A[5] = 5.

示例 2：

输入：[9,8,1,0,1,9,4,0,4,1]
输出：7
解释：
最大宽度的坡为 (i, j) = (2, 9): A[2] = 1 且 A[9] = 1.

__提示:__

2 <= A.length <= 50000
0 <= A[i] <= 50000

__思路__:

1. 排序
将下标列表按照 nums 中得对应大小进行排序, 注意需要用稳定排序或者自定义排序保证小于或等于关系
遍历下标列表
用一个 min_value 记录当前最小值
更新结果为最大的当前值减去 min_value
时间复杂度为 O(nlgn), 空间复杂度为 O(n)
2. 二分查找
从后往前遍历
如果二分查找的结果小于 candidates 说明有满足题意的 i 使得 nums[j] >= nums[i], 且 j > i, 更新结果
否则将这个位置加入到 candidates 列表中
结果为 candidates[pos][1] - i 的最大值
时间复杂度为 O(nlgn), 空间复杂度为 O(n)
3. 单调栈
先将一个严格单调递减的数列插入到单调栈中, 如果不是严格单调递减, 比如 [1, 2] 这种情况只要选择 2 一定比 1 更差, 因为 1 对应的下标也比 2 小, 所以 2 一定不用考虑
逆序查找最长的坡, 弹出栈中元素直到栈顶对应的元素大于当前元素, 逆序是用贪心的方式, 可以方便快速找到当前最长的坡
更新结果为最大的当前位置和栈顶的差值
时间复杂度为 O(n), 空间复杂度为 O(n), 每个元素最多入栈出栈一次

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maxWidthRamp(vector<int>& nums)
    {
        int n = nums.size(), result = 0;
        stack<int> s;
        for (int i = 0; i < n; i++) if (s.empty() or nums[s.top()] > nums[i]) s.push(i);
        for (int i = n - 1; i > -1; i--)
        {
            while (!s.empty() and nums[s.top()] <= nums[i])
            {
                result = max(result, i - s.top());
                s.pop();
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxWidthRamp(int[] nums) {
        int n = nums.length, minValue = n, result = 0;
        Integer parent[] = new Integer[n];
        for (int i = 0; i < n; i++) parent[i] = i;
        Arrays.sort(parent, (a, b) -> ((Integer)nums[a]).compareTo(nums[b]));
        for (int num : parent) {
            result = Math.max(result, num - minValue);
            minValue = Math.min(minValue, num);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxWidthRamp(self, nums: List[int]) -> int:
        result, candidates = 0, [(nums[(n := len(nums)) - 1], n - 1)]
        for i in range(n - 2, -1, -1):
            if (pos := bisect.bisect(candidates, (nums[i],))) < len(candidates):
                result = max(result, candidates[pos][1] - i)
            else:
                candidates.append((nums[i], i))
        return result
```
