# 3038 Maximum Number of Operations With the Same Score I 相同分数的最大操作数目 I

__Description:__

You are given an array of integers `nums`. Consider the following operation:

- Delete the first two elements `nums` and define the _score_ of the operation as the sum of these two elements.

You can perform this operation until `nums` contains fewer than two elements. Additionally, the __same__ _score_ must be achieved in __all__ operations.

Return the maximum number of operations you can perform.

__Example:__

Example 1:

```text
Input: nums = [3,2,1,4,5]

Output: 2

Explanation:
```

- We can perform the first operation with the score `3 + 2 = 5`. After this operation, `nums = [1,4,5]`.
- We can perform the second operation as its score is `4 + 1 = 5`, the same as the previous operation. After this operation, `nums = [5]`.
- As there are fewer than two elements, we can't perform more operations.

Example 2:

```text
Input: nums = [1,5,3,3,4,1,3,2,2,3]

Output: 2

Explanation:
```

- We can perform the first operation with the score `1 + 5 = 6`. After this operation, `nums = [3,3,4,1,3,2,2,3]`.
- We can perform the second operation as its score is `3 + 3 = 6`, the same as the previous operation. After this operation, `nums = [4,1,3,2,2,3]`.
- We cannot perform the next operation as its score is `4 + 1 = 5`, which is different from the previous scores.

Example 3:

```text
Input: nums = [5,3]

Output: 1
```

__Constraints:__

- `2 <= nums.length <= 100`
- `1 <= nums[i] <= 1000`

__题目描述:__

给你一个整数数组 `nums` ，如果 `nums` __至少__ 包含 `2` 个元素，你可以执行以下操作:

- 选择 `nums` 中的前两个元素并将它们删除。

一次操作的 分数 是被删除元素的和。

在确保 所有操作分数相同 的前提下，请你求出 最多 能进行多少次操作。

请你返回按照上述要求 最多 可以进行的操作次数。

__示例:__

示例 1：

```text
输入：nums = [3,2,1,4,5]
输出：2
解释：我们执行以下操作：
```

- 删除前两个元素，分数为 3 + 2 = 5 ，nums = [1,4,5] 。
- 删除前两个元素，分数为 1 + 4 = 5 ，nums = [5] 。

由于只剩下 1 个元素，我们无法继续进行任何操作。

示例 2：

```text
输入：nums = [3,2,6,1,4]
输出：1
解释：我们执行以下操作：
```

- 删除前两个元素，分数为 3 + 2 = 5 ，nums = [6,1,4] 。

由于下一次操作的分数与前一次不相等，我们无法继续进行任何操作。

__提示：__

- `2 <= nums.length <= 100`
- `1 <= nums[i] <= 1000`

__思路:__

```text
模拟
先计算前两个数的和
如果满足相邻两个数和相等就继续
否则直接返回 i >> 1
最后如果遍历完数组返回 n >> 1
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxOperations(vector<int>& nums) 
    {
        int s = nums[0] + nums[1], n = nums.size();
        for (int i = 3; i < n; i += 2) if (nums[i - 1] + nums[i] != s) return i >> 1;
        return n >> 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxOperations(int[] nums) {
        int s = nums[0] + nums[1], n = nums.length;
        for (int i = 3; i < n; i += 2) if (nums[i - 1] + nums[i] != s) return i >>> 1;
        return n >>> 1;
    }
}
```

__Python__:

```Python
class Solution:
    def maxOperations(self, nums: List[int]) -> int:
        s, n = nums[0] + nums[1], len(nums)
        for i in range(3, n, 2):
            if nums[i - 1] + nums[i] != s:
                return i >> 1
        return n >> 1
```
