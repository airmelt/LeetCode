# 3066 Minimum Operations to Exceed Threshold Value II 超过阈值的最少操作数 II

__Description:__

You are given a __0-indexed__ integer array `nums`, and an integer `k`.

You are allowed to perform some operations on `nums`, where in a single operation, you can:

- Select the two __smallest__ integers `x` and `y` from `nums`.
- Remove `x` and `y` from `nums`.
- Insert `(min(x, y) * 2 + max(x, y))` at any position in the array.

__Note__ that you can only apply the described operation if `nums` contains __at least__ two elements.

Return the __minimum__ number of operations needed so that all elements of the array are __greater than or equal to__ `k`.

__Example:__

Example 1:

```text
Input: nums = [2,11,10,1,3], k = 10

Output: 2

Explanation:

At this stage, all the elements of nums are greater than or equal to 10 so we can stop.

It can be shown that 2 is the minimum number of operations needed so that all elements of the array are greater than or equal to 10.
```

Example 2:

```text
Input: nums = [1,1,2,4,9], k = 20

Output: 4

Explanation:

At this stage, all the elements of nums are greater than 20 so we can stop.

It can be shown that 4 is the minimum number of operations needed so that all elements of the array are greater than or equal to 20.
```

__Constraints:__

- `2 <= nums.length <= 2 * 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= k <= 10 ^ 9`
- The input is generated such that an answer always exists. That is, after performing some number of operations, all elements of the array are greater than or equal to `k`.

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 和一个整数 `k` 。

你可以对 `nums` 执行一些操作，在一次操作中，你可以:

- 选择 `nums` 中 __最小__ 的两个整数 `x` 和 `y` 。
- 将 `x` 和 `y` 从 `nums` 中删除。
- 将 `min(x, y) * 2 + max(x, y)` 添加到数组中的任意位置。

_注意，_ 只有当 `nums` __至少__ 包含两个元素时，你才可以执行以上操作。

你需要使数组中的所有元素都 __大于或等于__ `k` ，请你返回需要的 __最少__ 操作次数。

__示例:__

示例 1：

```text
输入：nums = [2,11,10,1,3], k = 10

输出：2

解释：

此时，数组中的所有元素都大于等于 10 ，所以我们停止操作。

可以证明使数组中所有元素都大于等于 10 需要的最少操作次数为 2 。
```

示例 2：

```text
输入：nums = [1,1,2,4,9], k = 20

输出：4

解释：

此时， nums 中的所有元素都大于等于 20 ，所以我们停止操作。

可以证明使数组中所有元素都大于等于 20 需要的最少操作次数为 4 。
```

__提示：__

- `2 <= nums.length <= 2 * 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= k <= 10 ^ 9`
- 输入保证答案一定存在，也就是说，在进行某些次数的操作后，数组中所有元素都大于等于 `k` 。

__思路:__

```text
优先队列模拟
使用小根堆
每次取出最小的两个元素 x, y, 即依次弹出堆顶两个元素
然后将 (x << 1) + y 加入小根堆
直到小根堆堆顶不小于 k
时间复杂度为 O(NlogN), 空间复杂度为 O(N), 原地堆化可以将空间复杂度降低至 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(vector<int>& nums, int k) 
    {
        priority_queue<long long, vector<long long>, greater<>> pq(nums.begin(), nums.end());
        int result = 0;
        while (pq.top() < k) 
        {
            long long x = pq.top(); 
            pq.pop();
            long long y = pq.top(); 
            pq.pop();
            pq.push((x << 1LL) + y);
            ++result;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(int[] nums, int k) {
        int result = 0;
        var pq = new PriorityQueue<Long>();
        for (int num : nums) pq.offer((long)num);
        while (pq.peek() < k) {
            long x = pq.poll(), y = pq.poll();
            pq.offer((x << 1L) + y);
            ++result;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, nums: List[int], k: int) -> int:
        heapify(nums)
        result = 0
        while nums[0] < k:
            x = heappop(nums)
            heapreplace(nums, nums[0] + (x << 1))
            result += 1
        return result
```
