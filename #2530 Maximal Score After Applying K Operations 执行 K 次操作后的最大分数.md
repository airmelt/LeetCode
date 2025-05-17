# 2530 Maximal Score After Applying K Operations 执行 K 次操作后的最大分数

__Description:__

You are given a __0-indexed__ integer array `nums` and an integer `k`. You have a __starting score__ of `0`.

In one operation:

Return _the maximum possible __score__ you can attain after applying __exactly___ `k` _operations_.

The ceiling function `ceil(val)` is the least integer greater than or equal to `val`.

__Example:__

Example 1:

```text
Input: nums = [10,10,10,10,10], k = 5
Output: 50
Explanation: Apply the operation to each array element exactly once. The final score is 10 + 10 + 10 + 10 + 10 = 50.
```

Example 2:

```text
Input: nums = [1,10,3,3,3], k = 3
Output: 17
Explanation: You can do the following operations:
Operation 1: Select i = 1, so nums becomes [1,4,3,3,3]. Your score increases by 10.
Operation 2: Select i = 1, so nums becomes [1,2,3,3,3]. Your score increases by 4.
Operation 3: Select i = 2, so nums becomes [1,2,1,3,3]. Your score increases by 3.
The final score is 10 + 4 + 3 = 17.
```

__Constraints:__

- `1 <= nums.length, k <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 和一个整数 `k` 。你的 __起始分数__ 为 `0` 。

在一步 操作 中：

返回在 __恰好__ 执行 `k` 次操作后，你可能获得的最大分数。

向上取整函数 `ceil(val)` 的结果是大于或等于 `val` 的最小整数。

__示例:__

示例 1：

```text
输入：nums = [10,10,10,10,10], k = 5
输出：50
解释：对数组中每个元素执行一次操作。最后分数是 10 + 10 + 10 + 10 + 10 = 50 。
```

示例 2：

```text
输入：nums = [1,10,3,3,3], k = 3
输出：17
解释：可以执行下述操作：
第 1 步操作：选中 i = 1 ，nums 变为 [1,4,3,3,3] 。分数增加 10 。
第 2 步操作：选中 i = 1 ，nums 变为 [1,2,3,3,3] 。分数增加 4 。
第 3 步操作：选中 i = 2 ，nums 变为 [1,2,1,3,3] 。分数增加 3 。
最后分数是 10 + 4 + 3 = 17 。
```

__提示：__

- `1 <= nums.length, k <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
优先队列
将数组原地堆化
每次取出最大数
并把它替换为 ceil(nums[i] / 3)
然后将它放回堆中
累计 k 个最大值
时间复杂度为 O(N + KlogN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maxKelements(vector<int> &nums, int k) 
    {
        make_heap(nums.begin(), nums.end());
        long long result = 0;
        while (k--) 
        {
            pop_heap(nums.begin(), nums.end());
            result += nums.back();
            nums.back() = (nums.back() + 2) / 3;
            push_heap(nums.begin(), nums.end());
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long maxKelements(int[] nums, int k) {
        PriorityQueue<Long> pq = new PriorityQueue<Long>((a, b) -> (int)(b - a));
        for (int num : nums) pq.offer((long)num);
        long result = 0L, x = 0L;
        while ((k--) > 0) {
            result += (x = pq.poll());
            pq.offer((x + 2L) / 3L);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxKelements(self, nums: List[int], k: int) -> int:
        heapify(nums := [-i for i in nums])
        result = 0
        for _ in range(k):
            result -= heapreplace(nums, nums[0] // 3)
        return result
```
