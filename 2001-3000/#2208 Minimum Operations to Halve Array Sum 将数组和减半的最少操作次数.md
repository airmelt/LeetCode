# 2208 Minimum Operations to Halve Array Sum 将数组和减半的最少操作次数

__Description:__

You are given an array `nums` of positive integers. In one operation, you can choose __any__ number from `nums` and reduce it to __exactly__ half the number. (Note that you may choose this reduced number in future operations.)

Return _the __minimum__ number of operations to reduce the sum of_ `nums` _by __at least__ half._

__Example:__

Example 1:

```text
Input: nums = [5,19,8,1]
Output: 3
Explanation: The initial sum of nums is equal to 5 + 19 + 8 + 1 = 33.
The following is one of the ways to reduce the sum by at least half:
Pick the number 19 and reduce it to 9.5.
Pick the number 9.5 and reduce it to 4.75.
Pick the number 8 and reduce it to 4.
The final array is [5, 4.75, 4, 1] with a total sum of 5 + 4.75 + 4 + 1 = 14.75. 
The sum of nums has been reduced by 33 - 14.75 = 18.25, which is at least half of the initial sum, 18.25 >= 33/2 = 16.5.
Overall, 3 operations were used so we return 3.
It can be shown that we cannot reduce the sum by at least half in less than 3 operations.
```

Example 2:

```text
Input: nums = [3,8,20]
Output: 3
Explanation: The initial sum of nums is equal to 3 + 8 + 20 = 31.
The following is one of the ways to reduce the sum by at least half:
Pick the number 20 and reduce it to 10.
Pick the number 10 and reduce it to 5.
Pick the number 3 and reduce it to 1.5.
The final array is [1.5, 8, 5] with a total sum of 1.5 + 8 + 5 = 14.5. 
The sum of nums has been reduced by 31 - 14.5 = 16.5, which is at least half of the initial sum, 16.5 >= 31/2 = 15.5.
Overall, 3 operations were used so we return 3.
It can be shown that we cannot reduce the sum by at least half in less than 3 operations.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 7`

__题目描述:__

给你一个正整数数组 `nums` 。每一次操作中，你可以从 `nums` 中选择 __任意__ 一个数并将它减小到 __恰好__ 一半。（注意，在后续操作中你可以对减半过的数继续执行操作）

请你返回将 `nums` 数组和 __至少__ 减少一半的 __最少__ 操作数。

__示例:__

示例 1：

```text
输入：nums = [5,19,8,1]
输出：3
解释：初始 nums 的和为 5 + 19 + 8 + 1 = 33 。
以下是将数组和减少至少一半的一种方法：
选择数字 19 并减小为 9.5 。
选择数字 9.5 并减小为 4.75 。
选择数字 8 并减小为 4 。
最终数组为 [5, 4.75, 4, 1] ，和为 5 + 4.75 + 4 + 1 = 14.75 。
nums 的和减小了 33 - 14.75 = 18.25 ，减小的部分超过了初始数组和的一半，18.25 >= 33/2 = 16.5 。
我们需要 3 个操作实现题目要求，所以返回 3 。
可以证明，无法通过少于 3 个操作使数组和减少至少一半。
```

示例 2：

```text
输入：nums = [3,8,20]
输出：3
解释：初始 nums 的和为 3 + 8 + 20 = 31 。
以下是将数组和减少至少一半的一种方法：
选择数字 20 并减小为 10 。
选择数字 10 并减小为 5 。
选择数字 3 并减小为 1.5 。
最终数组为 [1.5, 8, 5] ，和为 1.5 + 8 + 5 = 14.5 。
nums 的和减小了 31 - 14.5 = 16.5 ，减小的部分超过了初始数组和的一半， 16.5 >= 31/2 = 15.5 。
我们需要 3 个操作实现题目要求，所以返回 3 。
可以证明，无法通过少于 3 个操作使数组和减少至少一半。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 7`

__思路:__

```text
贪心 ➕ 优先队列
将所有元素放入优先队列中, 每次取出最大的元素, 将其减半, 并将其重新放入优先队列中
如果减半后, 累计和仍然大于 0, 则继续循环
最终返回循环的次数即可
时间复杂度为 O(NlogN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int halveArray(vector<int>& nums) 
    {
        priority_queue<double> pq{ nums.begin(), nums.end() };
        double target = accumulate(nums.begin(), nums.end(), 0.0) / 2;
        int result = 0;
        while (target > 0) 
        {
            double cur = pq.top();
            pq.pop();
            target -= (cur /= 2.0);
            pq.push(cur);
            ++result;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int halveArray(int[] nums) {
        int result = 0;
        double target = Arrays.stream(nums).mapToDouble(f -> f).sum() / 2.0;
        PriorityQueue<Double> pq = new PriorityQueue<>((a, b) -> b.compareTo(a));
        for (int num : nums) pq.offer(num * 1.0);
        while (target > 0) {
            double cur = pq.poll() / 2.0;
            target -= cur;
            pq.offer(cur);
            ++result;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def halveArray(self, nums: List[int]) -> int:
        target, result = (s := sum(nums)) / 2.0, 0
        heapify((nums := [-num for num in nums]))
        while target > 0:
            target -= (cur := -heappop(nums)) / 2.0
            heappush(nums, -cur / 2.0)
            result += 1
        return result
```
