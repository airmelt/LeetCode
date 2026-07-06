# 3091 Apply Operations to Make Sum of Array Greater Than or Equal to k 执行操作使数据元素之和大于等于 K

__Description:__

You are given a __positive__ integer `k`. Initially, you have an array `nums = [1]`.

You can perform any of the following operations on the array any number of times (possibly zero):

- Choose any element in the array and __increase__ its value by `1`.
- Duplicate any element in the array and add it to the end of the array.

Return _the __minimum__ number of operations required to make the __sum__ of elements of the final array greater than or equal to_ `k`.

__Example:__

Example 1:

```text
Input: k = 11

Output: 5

Explanation:
```

We can do the following operations on the array `nums = [1]`:

- Increase the element by `1` three times. The resulting array is `nums = [4]`.
- Duplicate the element two times. The resulting array is `nums = [4,4,4]`.

The sum of the final array is `4 + 4 + 4 = 12` which is greater than or equal to `k = 11`.

The total number of operations performed is `3 + 2 = 5`.

Example 2:

```text
Input: k = 1

Output: 0

Explanation:
```

The sum of the original array is already greater than or equal to `1`, so no operations are needed.

__Constraints:__

- `1 <= k <= 10 ^ 5`

__题目描述:__

给你一个__正整数__ `k` 。最初，你有一个数组 `nums = [1]` 。

你可以对数组执行以下 任意 操作 任意 次数（可能为零）：

- 选择数组中的任何一个元素，然后将它的值 __增加__ `1` 。
- 复制数组中的任何一个元素，然后将它附加到数组的末尾。

返回使得最终数组元素之 __和__ 大于或等于 `k` 所需的 __最少__ 操作次数。

__示例:__

示例 1：

```text
输入：k = 11

输出：5

解释：
```

可以对数组 `nums = [1]` 执行以下操作:

- 将元素的值增加 `1` 三次。结果数组为 `nums = [4]` 。
- 复制元素两次。结果数组为 `nums = [4,4,4]` 。

最终数组的和为 `4 + 4 + 4 = 12` ，大于等于 `k = 11` 。

执行的总操作次数为 `3 + 2 = 5` 。

示例 2：

```text
输入：k = 1

输出：0

解释：
```

原始数组的和已经大于等于 `1` ，因此不需要执行操作。

__提示：__

- `1 <= k <= 10 ^ 5`

__思路:__

```text
1. 贪心
如果要使用 add 次加法
那么最大值为 1 + add = m
再进行复制还需要 k / m - 1 向上取整次操作
总步骤为 m - 1 + (k - 1) / m
从 [1, k] 枚举 m 取最小值
时间复杂度为 O(N), 空间复杂度为 O(1)
2. 数学
由对勾函数最小值可得
当 m = (k - 1) ^ 1 / 2 时取到最小值
所以 (k - 1) ^ 1 / 2 或者 (k - 1) ^ 1 / 2 + 1 为最小值
代入 m - 1 + (k - 1) / m 求解即可
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(int k) 
    {
        return min(max((int)sqrt(k - 1), 1) - 1 + (k - 1) / max((int)sqrt(k - 1), 1), max((int)sqrt(k - 1), 1) + (k - 1) / (max((int)sqrt(k - 1), 1) + 1));
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(int k) {
        return IntStream.rangeClosed(1, k).map(m -> m - 1 + (k - 1) / m).min().getAsInt();
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, k: int) -> int:
        return min(m - 1 + (k - 1) // m for m in range(1, k + 1))
```
