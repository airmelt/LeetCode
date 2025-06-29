# 2578 Split With Minimum Sum 最小和分割

__Description:__

Given a positive integer `num`, split it into two non-negative integers `num1` and `num2` such that:

- The concatenation of `num1` and `num2` is a permutation of `num`.
- In other words, the sum of the number of occurrences of each digit in `num1` and `num2` is equal to the number of occurrences of that digit in `num`.
- `num1` and `num2` can contain leading zeros.

Return _the __minimum__ possible sum of_ `num1` _and_ `num2`.

__Notes:__

- It is guaranteed that `num` does not contain any leading zeros.
- The order of occurrence of the digits in `num1` and `num2` may differ from the order of occurrence of `num`.

__Example:__

Example 1:

```text
Input:  num = 4325
Output:  59
Explanation:  We can split 4325 so that num1 is 24 and num2 is 35, giving a sum of 59. We can prove that 59 is indeed the minimal possible sum.
```

Example 2:

```text
Input:  num = 687
Output:  75
Explanation:  We can split 687 so that num1 is 68 and num2 is 7, which would give an optimal sum of 75.
```

__Constraints:__

- `10 <= num <= 10 ^ 9`

__题目描述:__

给你一个正整数 `num` ，请你将它分割成两个非负整数 `num1` 和 `num2` ，满足:

- `num1` 和 `num2` 直接连起来，得到 `num` 各数位的一个排列。
- 换句话说， `num1` 和 `num2` 中所有数字出现的次数之和等于 `num` 中所有数字出现的次数。
- `num1` 和 `num2` 可以包含前导 0 。

请你返回 `num1` 和 `num2` 可以得到的和的 __最小__ 值。

__注意：__

- `num` 保证没有前导 0 。
- `num1` 和 `num2` 中数位顺序可以与 `num` 中数位顺序不同。

__示例:__

示例 1：

```text
输入: num = 4325
输出: 59
解释: 我们可以将 4325 分割成 num1 = 24 和 num2 = 35 ，和为 59 ，59 是最小和。
```

示例 2：

```text
输入: num = 687
输出: 75
解释: 我们可以将 687 分割成 num1 = 68 和 num2 = 7 ，和为最优值 75 。
```

__提示：__

- `10 <= num <= 10 ^ 9`

__思路:__

```text
贪心
先将所有数字按照从小到大排序
排序之后分别将奇数位置上的数字分入 num1, 偶数位置上的数字分入 num2
这样可以保证 num1 和 num2 的和最小
时间复杂度为 O(MlogM), 空间复杂度为 O(M), 其中 M 为 num 的位数, 即 logN
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int splitNum(int num) 
    {
        string s = to_string(num);
        ranges::sort(s);
        int result[2]{}, n = s.size();
        for (int i = 0; i < n; i++) result[i & 1] = result[i & 1] * 10 + s[i] - '0';
        return result[0] + result[1];
    }
};
```

__Java__:

```Java
class Solution {
    public int splitNum(int num) {
        char[] s = Integer.toString(num).toCharArray();
        Arrays.sort(s);
        int result[] = new int[2], n = s.length;
        for (int i = 0; i < s.length; i++) result[i & 1] = result[i & 1] * 10 + s[i] - '0';
        return result[0] + result[1];
    }
}
```

__Python__:

```Python
class Solution:
    def splitNum(self, num: int) -> int:
        return 0 if not (a := "".join(sorted(str(num)))) else int(a[::2]) + int(a[1::2])
```
