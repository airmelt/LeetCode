# 2341 Maximum Number of Pairs in Array 数组能形成多少数对

__Description:__

You are given a __0-indexed__ integer array `nums`. In one operation, you may do the following:

- Choose __two__ integers in `nums` that are __equal__.
- Remove both integers from `nums`, forming a __pair__.

The operation is done on `nums` as many times as possible.

Return _a __0-indexed__ integer array_ `answer` _of size_ `2` _where_ `answer[0]` _is the number of pairs that are formed and_ `answer[1]` _is the number of leftover integers in_ `nums` _after doing the operation as many times as possible_.

__Example:__

Example 1:

```text
Input: nums = [1,3,2,1,3,2,2]
Output: [3,1]
Explanation:
Form a pair with nums[0] and nums[3] and remove them from nums. Now, nums = [3,2,3,2,2].
Form a pair with nums[0] and nums[2] and remove them from nums. Now, nums = [2,2,2].
Form a pair with nums[0] and nums[1] and remove them from nums. Now, nums = [2].
No more pairs can be formed. A total of 3 pairs have been formed, and there is 1 number leftover in nums.
```

Example 2:

```text
Input: nums = [1,1]
Output: [1,0]
Explanation: Form a pair with nums[0] and nums[1] and remove them from nums. Now, nums = [].
No more pairs can be formed. A total of 1 pair has been formed, and there are 0 numbers leftover in nums.
```

Example 3:

```text
Input: nums = [0]
Output: [0,1]
Explanation: No pairs can be formed, and there is 1 number leftover in nums.
```

__Constraints:__

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 100`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。在一步操作中，你可以执行以下步骤:

- 从 `nums` 选出 __两个__ __相等的__ 整数
- 从 `nums` 中移除这两个整数，形成一个 __数对__

请你在 `nums` 上多次执行此操作直到无法继续执行。

返回一个下标从 __0__ 开始、长度为 `2` 的整数数组 `answer` 作为答案，其中 `answer[0]` 是形成的数对数目， `answer[1]` 是对 `nums` 尽可能执行上述操作后剩下的整数数目。

__示例:__

示例 1：

```text
输入：nums = [1,3,2,1,3,2,2]
输出：[3,1]
解释：
nums[0] 和 nums[3] 形成一个数对，并从 nums 中移除，nums = [3,2,3,2,2] 。
nums[0] 和 nums[2] 形成一个数对，并从 nums 中移除，nums = [2,2,2] 。
nums[0] 和 nums[1] 形成一个数对，并从 nums 中移除，nums = [2] 。
无法形成更多数对。总共形成 3 个数对，nums 中剩下 1 个数字。
```

示例 2：

```text
输入：nums = [1,1]
输出：[1,0]
解释：nums[0] 和 nums[1] 形成一个数对，并从 nums 中移除，nums = [] 。
无法形成更多数对。总共形成 1 个数对，nums 中剩下 0 个数字。
```

示例 3：

```text
输入：nums = [0]
输出：[0,1]
解释：无法形成数对，nums 中剩下 1 个数字。
```

__提示：__

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 100`

__思路:__

```text
模拟
用哈希表或者布尔数组记录每个数字是否出现过
出现之后将布尔值取反, 如果取反之后为真, 则说明这个数字出现过, 数对数目加一
最后返回数对数目和剩下的数字数目
时间复杂度为 O(N), 空间复杂度为 O(M), 其中 M 为数组中不同数字的个数
```

__代码:__

__C++__:

```C++
class Solution {
    public int[] numberOfPairs(int[] nums) {
        boolean[] visited = new boolean[101]; 
        int pairs = 0;
        for (int num : nums) {
            if (visited[num]) ++pairs;
            visited[num] = !visited[num];
        }
        return new int[]{pairs, nums.length - (pairs << 1)};
    }
}
```

__Java__:

```Java
class Solution {
    public int[] numberOfPairs(int[] nums) {
        boolean[] visited = new boolean[101]; 
        int pairs = 0;
        for (int num : nums) {
            if (visited[num]) ++pairs;
            visited[num] = !visited[num];
        }
        return new int[]{pairs, nums.length - (pairs << 1)};
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfPairs(self, nums: List[int]) -> List[int]:
        return [sum(v >> 1 for v in Counter(nums).values()), sum(v & 1 for v in Counter(nums).values())]
```
