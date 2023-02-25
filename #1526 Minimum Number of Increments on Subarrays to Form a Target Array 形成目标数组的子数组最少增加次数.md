# 1526 Minimum Number of Increments on Subarrays to Form a Target Array 形成目标数组的子数组最少增加次数

__Description:__

You are given an integer array `target`. You have an integer array `initial` of the same size as `target` with all elements initially zeros.

In one operation you can choose __any__ subarray from `initial` and increment each value by one.

Return _the minimum number of operations to form a_ `target` _array from_ `initial`.

The test cases are generated so that the answer fits in a 32-bit integer.

__Example:__

Example 1:

```text
Input: target = [1,2,3,2,1]
Output: 3
Explanation: We need at least 3 operations to form the target array from the initial array.
[0,0,0,0,0] increment 1 from index 0 to 4 (inclusive).
[1,1,1,1,1] increment 1 from index 1 to 3 (inclusive).
[1,2,2,2,1] increment 1 at index 2.
[1,2,3,2,1] target array is formed.
```

Example 2:

```text
Input: target = [3,1,1,2]
Output: 4
Explanation: [0,0,0,0] -> [1,1,1,1] -> [1,1,1,2] -> [2,1,1,2] -> [3,1,1,2]
```

Example 3:

```text
Input: target = [3,1,5,4,2]
Output: 7
Explanation: [0,0,0,0,0] -> [1,1,1,1,1] -> [2,1,1,1,1] -> [3,1,1,1,1] -> [3,1,2,2,2] -> [3,1,3,3,2] -> [3,1,4,4,2] -> [3,1,5,4,2].
```

__Constraints:__

- `1 <= target.length <= 10 ^ 5`
- `1 <= target[i] <= 10 ^ 5`

__题目描述:__

给你一个整数数组 `target` 和一个数组 `initial` ， `initial` 数组与 `target` 数组有同样的维度，且一开始全部为 0 。

请你返回从 `initial` 得到  `target` 的最少操作次数，每次操作需遵循以下规则：

- 在 `initial` 中选择 __任意__ 子数组，并将子数组中每个元素增加 1 。

答案保证在 32 位有符号整数以内。

__示例:__

示例 1：

```text
输入：target = [1,2,3,2,1]
输出：3
解释：我们需要至少 3 次操作从 intial 数组得到 target 数组。
[0,0,0,0,0] 将下标为 0 到 4 的元素（包含二者）加 1 。
[1,1,1,1,1] 将下标为 1 到 3 的元素（包含二者）加 1 。
[1,2,2,2,1] 将下表为 2 的元素增加 1 。
[1,2,3,2,1] 得到了目标数组。
```

示例 2：

```text
输入：target = [3,1,1,2]
输出：4
解释：(initial)[0,0,0,0] -> [1,1,1,1] -> [1,1,1,2] -> [2,1,1,2] -> [3,1,1,2] (target) 。
```

示例 3：

```text
输入：target = [3,1,5,4,2]
输出：7
解释：(initial)[0,0,0,0,0] -> [1,1,1,1,1] -> [2,1,1,1,1] -> [3,1,1,1,1] -> [3,1,2,2,2] -> [3,1,3,3,2] -> [3,1,4,4,2] -> [3,1,5,4,2] (target)。
```

示例 4：

```text
输入：target = [1,1,1,1]
输出：1
```

__提示：__

- `1 <= target.length <= 10 ^ 5`
- `1 <= target[i] <= 10 ^ 5`

__思路:__

```text
模拟
升序需要累积两个相邻元素的差值
降序可以和升序的同时累加
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minNumberOperations(vector<int>& target) 
    {
        int result = target.front(), n = target.size();
        for (int i = 1; i < n; i++) result += max(target[i] - target[i - 1], 0);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minNumberOperations(int[] target) {
        int result = target[0], n = target.length;
        for (int i = 1; i < n; i++) result += Math.max(target[i] - target[i - 1], 0);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minNumberOperations(self, target: List[int]) -> int:
          return sum([target[i + 1] - target[i] for i in range(len(target) - 1) if target[i] < target[i + 1]]) + target[0]
```
