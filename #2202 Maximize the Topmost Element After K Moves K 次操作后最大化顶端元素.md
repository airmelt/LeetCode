# 2202 Maximize the Topmost Element After K Moves K 次操作后最大化顶端元素

__Description:__

You are given a __0-indexed__ integer array `nums` representing the contents of a _pile_ , where `nums[0]` is the topmost element of the pile.

In one move, you can perform either of the following:

- If the pile is not empty, __remove__ the topmost element of the pile.
- If there are one or more removed elements, __add__ any one of them back onto the pile. This element becomes the new topmost element.

You are also given an integer `k`, which denotes the total number of moves to be made.

Return _the __maximum value__ of the topmost element of the pile possible after __exactly___ `k` _moves_. In case it is not possible to obtain a non-empty pile after `k` moves, return `-1`.

__Example:__

Example 1:

```text
Input: nums = [5,2,2,4,0,6], k = 4
Output: 5
Explanation:
One of the ways we can end with 5 at the top of the pile after 4 moves is as follows:
```

- Step 1: Remove the topmost element = 5. The pile becomes [2,2,4,0,6].
- Step 2: Remove the topmost element = 2. The pile becomes [2,4,0,6].
- Step 3: Remove the topmost element = 2. The pile becomes [4,0,6].
- Step 4: Add 5 back onto the pile. The pile becomes [5,4,0,6].

Note that this is not the only way to end with 5 at the top of the pile. It can be shown that 5 is the largest answer possible after 4 moves.

Example 2:

```text
Input: nums = [2], k = 1
Output: -1
Explanation: 
In the first move, our only option is to pop the topmost element of the pile.
Since it is not possible to obtain a non-empty pile after one move, we return -1.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i], k <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，它表示一个 __栈__ ，其中 `nums[0]` 是栈顶的元素。

每一次操作中，你可以执行以下操作 之一 ：

- 如果栈非空，那么 __删除__ 栈顶端的元素。
- 如果存在 1 个或者多个被删除的元素，你可以从它们中选择任何一个， _添加_ 回栈顶，这个元素成为新的栈顶元素。

同时给你一个整数 `k` ，它表示你总共需要执行操作的次数。

请你返回 __恰好__ 执行 `k` 次操作以后，栈顶元素的 __最大值__ 。如果执行完 `k` 次操作以后，栈一定为空，请你返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums = [5,2,2,4,0,6], k = 4
输出：5
解释：
4 次操作后，栈顶元素为 5 的方法之一为：
```

- 第 1 次操作：删除栈顶元素 5 ，栈变为 [2,2,4,0,6] 。
- 第 2 次操作：删除栈顶元素 2 ，栈变为 [2,4,0,6] 。
- 第 3 次操作：删除栈顶元素 2 ，栈变为 [4,0,6] 。
- 第 4 次操作：将 5 添加回栈顶，栈变为 [5,4,0,6] 。

注意，这不是最后栈顶元素为 5 的唯一方式。但可以证明，4 次操作以后 5 是能得到的最大栈顶元素。

示例 2：

```text
输入：nums = [2], k = 1
输出：-1
解释：
第 1 次操作中，我们唯一的选择是将栈顶元素弹出栈。
由于 1 次操作后无法得到一个非空的栈，所以我们返回 -1 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i], k <= 10 ^ 9`

__思路:__

```text
分类讨论
如果 nums 的长度为 1, 且 k 为奇数, 则返回 -1
否则返回 nums 中前 k 个元素中的最大值
注意到不能选择 k - 1 位置的元素, 所以我们可以直接遍历 nums, 如果 i < k - 1 或者 i == k, 则返回 nums[i] 的最大值
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumTop(vector<int>& nums, int k) 
    {
        int result = -1, n = nums.size();
        if (n == 1 and (k & 1)) return result;
        for (int i = 0; i < n and i <= k; i++) if (i != k - 1) result = max(nums[i], result);
        return result; 
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumTop(int[] nums, int k) {
        int result = -1, n = nums.length;
        if (n == 1 && (k & 1) == 1) return result;
        for (int i = 0; i < n && i <= k; i++) if (i != k - 1) result = Math.max(nums[i], result);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumTop(self, nums: List[int], k: int) -> int:
        return -1 if (len(nums) == 1 and k & 1) else max(num for i, num in enumerate(nums) if i < k - 1 or i == k)
```
