# 1696 Jump Game VI 跳跃游戏VI

__Description:__

You are given a __0-indexed__ integer array `nums` and an integer `k`.

You are initially standing at index `0`. In one move, you can jump at most `k` steps forward without going outside the boundaries of the array. That is, you can jump from index `i` to any index in the range `[i + 1, min(n - 1, i + k)]` __inclusive__.

You want to reach the last index of the array (index `n - 1`). Your __score__ is the __sum__ of all `nums[j]` for each index `j` you visited in the array.

Return the maximum score you can get.

__Example:__

Example 1:

```text
Input: nums = [1,-1,-2,4,-7,3], k = 2
Output: 7
Explanation: You can choose your jumps forming the subsequence [1,-1,4,3] (underlined above). The sum is 7.
```

Example 2:

```text
Input: nums = [10,-5,-2,4,0,3], k = 3
Output: 17
Explanation: You can choose your jumps forming the subsequence [10,4,3] (underlined above). The sum is 17.
```

Example 3:

```text
Input: nums = [1,-5,-20,4,-1,3,-6,-3], k = 2
Output: 0
```

__Constraints:__

- `1 <= nums.length, k <= 10 ^ 5`
- `-10 ^ 4 <= nums[i] <= 10 ^ 4`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 和一个整数 `k` 。

一开始你在下标 `0` 处。每一步，你最多可以往前跳 `k` 步，但你不能跳出数组的边界。也就是说，你可以从下标 `i` 跳到 `[i + 1， min(n - 1, i + k)]` __包含__ 两个端点的任意位置。

你的目标是到达数组最后一个位置（下标为 `n - 1` ），你的 __得分__ 为经过的所有数字之和。

请你返回你能得到的 最大得分 。

__示例:__

示例 1：

```text
输入：nums = [1,-1,-2,4,-7,3], k = 2
输出：7
解释：你可以选择子序列 [1,-1,4,3] （上面加粗的数字），和为 7 。
```

示例 2：

```text
输入：nums = [10,-5,-2,4,0,3], k = 3
输出：17
解释：你可以选择子序列 [10,4,3] （上面加粗数字），和为 17 。
```

示例 3：

```text
输入：nums = [1,-5,-20,4,-1,3,-6,-3], k = 2
输出：0
```

__提示：__

- `1 <= nums.length, k <= 10 ^ 5`
- `-10 ^ 4 <= nums[i] <= 10 ^ 4`

__思路:__

```text
单调队列
记录每个位置的最大值, 每次从前 k 个位置中选择最大值跳到当前位置
上述算法的时间复杂度为 O(NK), 会超时
维护一个单调递减的队列，队首元素为当前最大值
每次只需要取出队首更新结果即可
如果队首元素不在当前位置的范围内，弹出队首元素
如果队尾元素小于当前位置的最大值，弹出队尾元素
时间复杂度为 O(N), 空间复杂度为 O(K)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxResult(vector<int>& nums, int k) 
    {
        int n = nums.size(), result = nums.front();
        deque<pair<int, int>> q{{nums.front(), 0}};
        for (int i = 1; i < n; i++)
        {
            if (i - q.front().second > k) q.pop_front();
            result = q.front().first + nums[i];
            while (!q.empty() and q.back().first <= result) q.pop_back();
            q.push_back({result, i});
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxResult(int[] nums, int k) {
        int result = nums[0], n = nums.length;
        Deque<int[]> deque = new ArrayDeque<>();
        deque.addLast(new int[]{nums[0], 0});
        for (int i = 1; i < n; i++) {
            if ((i - deque.getFirst()[1]) > k) deque.removeFirst();
            result = deque.getFirst()[0] + nums[i];
            while (!deque.isEmpty() && deque.getLast()[0] <= result) deque.removeLast();
            deque.addLast(new int[]{result, i});
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxResult(self, nums: List[int], k: int) -> int:
        result, q, n = nums[0], deque([[nums[0], 0]]), len(nums)
        for i in range(1, n):
            if i - q[0][1] > k:
                q.popleft()
            result = q[0][0] + nums[i]
            while q and q[-1][0] <= result:
                q.pop()
            q.append([result, i])
        return result
```
