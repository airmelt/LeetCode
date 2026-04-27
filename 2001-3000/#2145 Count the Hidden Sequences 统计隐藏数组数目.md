# 2145 Count the Hidden Sequences 统计隐藏数组数目

__Description:__

You are given a __0-indexed__ array of `n` integers `differences`, which describes the __differences__ between each pair of __consecutive__ integers of a __hidden__ sequence of length `(n + 1)`. More formally, call the hidden sequence `hidden`, then we have that `differences[i] = hidden[i + 1] - hidden[i]`.

You are further given two integers `lower` and `upper` that describe the __inclusive__ range of values `[lower, upper]` that the hidden sequence can contain.

- For example, given `differences = [1, -3, 4]`, `lower = 1`, `upper = 6`, the hidden sequence is a sequence of length `4` whose elements are in between `1` and `6` (__inclusive__).

  - `[3, 4, 1, 5]` and `[4, 5, 2, 6]` are possible hidden sequences.
  - `[5, 6, 3, 7]` is not possible since it contains an element greater than `6`.
  - `[1, 2, 3, 4]` is not possible since the differences are not correct.

Return _the number of __possible__ hidden sequences there are._ If there are no possible sequences, return `0`.

__Example:__

Example 1:

```text
Input: differences = [1,-3,4], lower = 1, upper = 6
Output: 2
Explanation: The possible hidden sequences are:
```

- [3, 4, 1, 5]
- [4, 5, 2, 6]

Thus, we return 2.

Example 2:

```text
Input: differences = [3,-4,5,1,-2], lower = -4, upper = 5
Output: 4
Explanation: The possible hidden sequences are:
```

- [-3, 0, -4, 1, 2, 0]
- [-2, 1, -3, 2, 3, 1]
- [-1, 2, -2, 3, 4, 2]
- [0, 3, -1, 4, 5, 3]

Thus, we return 4.

Example 3:

```text
Input: differences = [4,-7,2], lower = 3, upper = 6
Output: 0
Explanation: There are no possible hidden sequences. Thus, we return 0.
```

__Constraints:__

- `n == differences.length`
- `1 <= n <= 10 ^ 5`
- `-10 ^ 5 <= differences[i] <= 10 ^ 5`
- `-10 ^ 5 <= lower <= upper <= 10 ^ 5`

__题目描述:__

给你一个下标从 __0__ 开始且长度为 `n` 的整数数组 `differences` ，它表示一个长度为 `n + 1` 的 __隐藏__ 数组 __相邻__ 元素之间的 __差值__ 。更正式的表述为:我们将隐藏数组记作 `hidden` ，那么 `differences[i] = hidden[i + 1] - hidden[i]` 。

同时给你两个整数 `lower` 和 `upper` ，它们表示隐藏数组中所有数字的值都在 __闭__ 区间 `[lower, upper]` 之间。

- 比方说， `differences = [1, -3, 4]` ， `lower = 1` ， `upper = 6` ，那么隐藏数组是一个长度为 `4` 且所有值都在 `1` 和 `6` （包含两者）之间的数组。

  - `[3, 4, 1, 5]` 和 `[4, 5, 2, 6]` 都是符合要求的隐藏数组。
  - `[5, 6, 3, 7]` 不符合要求，因为它包含大于 `6` 的元素。
  - `[1, 2, 3, 4]` 不符合要求，因为相邻元素的差值不符合给定数据。

请你返回 __符合__ 要求的隐藏数组的数目。如果没有符合要求的隐藏数组，请返回 `0` 。

__示例:__

示例 1：

```text
输入：differences = [1,-3,4], lower = 1, upper = 6
输出：2
解释：符合要求的隐藏数组为：
```

- [3, 4, 1, 5]
- [4, 5, 2, 6]

所以返回 2 。

示例 2：

```text
输入：differences = [3,-4,5,1,-2], lower = -4, upper = 5
输出：4
解释：符合要求的隐藏数组为：
```

- [-3, 0, -4, 1, 2, 0]
- [-2, 1, -3, 2, 3, 1]
- [-1, 2, -2, 3, 4, 2]
- [0, 3, -1, 4, 5, 3]

所以返回 4 。

示例 3：

```text
输入：differences = [4,-7,2], lower = 3, upper = 6
输出：0
解释：没有符合要求的隐藏数组，所以返回 0 。
```

__提示：__

- `n == differences.length`
- `1 <= n <= 10 ^ 5`
- `-10 ^ 5 <= differences[i] <= 10 ^ 5`
- `-10 ^ 5 <= lower <= upper <= 10 ^ 5`

__思路:__

```text
模拟
differences 即差分数组
不妨设 a0 = 0
这样可以还原出 hidden 数组
需要保证 lower <= hidden[i] <= upper
即最小值 ai >= lower, 最大值 aj <= upper
即 aj - ai <= upper - ai -> ai <= upper - (aj - ai)
所以对所有的 ai 需要满足下界 lower, 上界 upper - (aj - ai)
其中 aj - ai = sum(differences[i:j]) 
由于只要求数组的个数
其实可以转化为求 a0 的取值范围
那么数组的数量为 (upper - lower) - (aj - ai) + 1
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfArrays(vector<int>& differences, int lower, int upper) 
    {
        int x = 0, y = 0, cur = 0;
        for (const auto& d : differences) 
        {
            cur += d;
            x = min(x, cur);
            y = max(y, cur);
            if (y - x > upper - lower) return 0;
        }
        return (upper - lower) - (y - x) + 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfArrays(int[] differences, int lower, int upper) {
        int x = 0, y = 0, cur = 0;
        for (int d : differences) {
            cur += d;
            x = Math.min(x, cur);
            y = Math.max(y, cur);
            if (y - x > upper - lower) return 0;
        }
        return (upper - lower) - (y - x) + 1;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfArrays(self, differences: List[int], lower: int, upper: int) -> int:
        return result if (result := (upper - lower) - (max(max(accumulate(differences)), 0) - min(min(accumulate(differences)), 0)) + 1) > 0 else 0
```
