# 2334 Subarray With Elements Greater Than Varying Threshold 元素值大于变化阈值的子数组

__Description:__

You are given an integer array `nums` and an integer `threshold`.

Find any subarray of `nums` of length `k` such that __every__ element in the subarray is __greater__ than `threshold / k`.

Return _the __size__ of __any__ such subarray_. If there is no such subarray, return `-1`.

A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [1,3,4,3,1], threshold = 6
Output: 3
Explanation: The subarray [3,4,3] has a size of 3, and every element is greater than 6 / 3 = 2.
Note that this is the only valid subarray.
```

Example 2:

```text
Input: nums = [6,5,6,5,8], threshold = 7
Output: 1
Explanation: The subarray [8] has a size of 1, and 8 > 7 / 1 = 7. So 1 is returned.
Note that the subarray [6,5] has a size of 2, and every element is greater than 7 / 2 = 3.5. 
Similarly, the subarrays [6,5,6], [6,5,6,5], [6,5,6,5,8] also satisfy the given conditions.
Therefore, 2, 3, 4, or 5 may also be returned.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i], threshold <= 10 ^ 9`

__题目描述:__

给你一个整数数组 `nums` 和一个整数 `threshold` 。

找到长度为 `k` 的 `nums` 子数组，满足数组中 __每个__ 元素都 __大于__ `threshold / k` 。

请你返回满足要求的 __任意__ 子数组的 __大小__ 。如果没有这样的子数组，返回 `-1` 。

子数组 是数组中一段连续非空的元素序列。

__示例:__

示例 1：

```text
输入：nums = [1,3,4,3,1], threshold = 6
输出：3
解释：子数组 [3,4,3] 大小为 3 ，每个元素都大于 6 / 3 = 2 。
注意这是唯一合法的子数组。
```

示例 2：

```text
输入：nums = [6,5,6,5,8], threshold = 7
输出：1
解释：子数组 [8] 大小为 1 ，且 8 > 7 / 1 = 7 。所以返回 1 。
注意子数组 [6,5] 大小为 2 ，每个元素都大于 7 / 2 = 3.5 。
类似的，子数组 [6,5,6] ，[6,5,6,5] ，[6,5,6,5,8] 都是符合条件的子数组。
所以返回 2, 3, 4 和 5 都可以。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i], threshold <= 10 ^ 9`

__思路:__

```text
1. 并查集
将数组中的元素按照从大到小排序, 并记录每个元素的下标
将 i 和 i + 1 连接, 并记录权重 weight[i + 1] = weight[i] + 1
如果 nums[i] > threshold // weight[i + 1], 则返回 weight[i + 1]
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
2. 单调栈
分别求出每个元素左边和右边第一个小于自己的元素的下标
left[i] 记录左边第一个小于自己的元素的下标, 不存在则为 -1
right[i] 记录右边第一个小于自己的元素的下标, 不存在则为 n
使用单调栈求出 left 和 right
令 k = right[i] - left[i] - 1
如果 nums[i] > threshold // k, 则返回 k
否则返回 -1
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int validSubarraySize(vector<int>& nums, int threshold) 
    {
        int n = nums.size();
        vector<int> left(n), right(n);
        stack<int> st;
        for (int i = 0; i < n; i++) 
        {
            while (!st.empty() and nums[st.top()] >= nums[i]) st.pop();
            left[i] = st.empty() ? -1 : st.top();
            st.push(i);
        }
        st = stack<int>();
        for (int i = n - 1; i > -1; i--) 
        {
            while (!st.empty() and nums[st.top()] >= nums[i]) st.pop();
            right[i] = st.empty() ? n : st.top();
            st.push(i);
        }
        for (int i = 0, k = 0; i < n; i++) if (nums[i] > threshold / (k = right[i] - left[i] - 1)) return k;
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int validSubarraySize(int[] nums, int threshold) {
        int n = nums.length, left[] = new int[n], right[] = new int[n];
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && nums[stack.peek()] >= nums[i]) stack.pop();
            left[i] = stack.isEmpty() ? -1 : stack.peek();
            stack.push(i);
        }
        stack.clear();
        for (int i = n - 1; i > -1; i--) {
            while (!stack.isEmpty() && nums[stack.peek()] >= nums[i]) stack.pop();
            right[i] = stack.isEmpty() ? n : stack.peek();
            stack.push(i);
        }
        for (int i = 0, k = 0; i < n; i++) if (nums[i] > threshold / (k = right[i] - left[i] - 1)) return k;
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def validSubarraySize(self, nums: List[int], threshold: int) -> int:
        weight, parent = [0] * ((n := len(nums)) + 1), list(range(n + 1))

        def find(x: int) -> int:
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]
        for num, i in sorted(zip(nums, range(n)), reverse=True):
            parent[i] = (j := find(i + 1))
            weight[j] += weight[i] + 1
            if num > threshold // weight[j]: 
                return weight[j]
        return -1
```
