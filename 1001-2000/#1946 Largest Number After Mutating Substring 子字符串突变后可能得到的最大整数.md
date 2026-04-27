# 1946 Largest Number After Mutating Substring 子字符串突变后可能得到的最大整数

__Description:__

You are given a string `num`, which represents a large integer. You are also given a __0-indexed__ integer array `change` of length `10` that maps each digit `0-9` to another digit. More formally, digit `d` maps to digit `change[d]`.

You may __choose__ to _mutate a single substring_ of `num`. To mutate a substring, replace each digit `num[i]` with the digit it maps to in `change` (i.e. replace `num[i]` with `change[num[i]]`).

Return _a string representing the __largest__ possible integer after __mutating__ (or choosing not to) a __single substring__ of_ `num`.

A substring is a contiguous sequence of characters within the string.

__Example:__

Example 1:

```text
Input: num = "132", change = [9,8,5,0,3,6,4,2,6,8]
Output: "832"
Explanation: Replace the substring "1":
```

- 1 maps to change[1] = 8.

Thus, "132" becomes "832".
"832" is the largest number that can be created, so return it.

Example 2:

```text
Input: num = "021", change = [9,4,3,5,7,2,1,9,0,6]
Output: "934"
Explanation: Replace the substring "021":
```

- 0 maps to change[0] = 9.
- 2 maps to change[2] = 3.
- 1 maps to change[1] = 4.

Thus, "021" becomes "934".
"934" is the largest number that can be created, so return it.

Example 3:

```text
Input: num = "5", change = [1,4,7,5,3,2,5,6,9,4]
Output: "5"
Explanation: "5" is already the largest number that can be created, so return it.
```

__Constraints:__

- `1 <= num.length <= 10 ^ 5`
- `num` consists of only digits `0-9`.
- `change.length == 10`
- `0 <= change[d] <= 9`

__题目描述:__

给你一个字符串 `num` ，该字符串表示一个大整数。另给你一个长度为 `10` 且 __下标从 0 开始__ 的整数数组 `change` ，该数组将 `0-9` 中的每个数字映射到另一个数字。更规范的说法是，数字 `d` 映射为数字 `change[d]` 。

你可以选择 __突变__  `num` 的任一子字符串。__突变__ 子字符串意味着将每位数字 `num[i]` 替换为该数字在 `change` 中的映射（也就是说，将 `num[i]` 替换为 `change[num[i]]`）。

请你找出在对 `num` 的任一子字符串执行突变操作（也可以不执行）后，可能得到的 __最大整数__ ，并用字符串表示返回。

子字符串 是字符串中的一个连续序列。

__示例:__

示例 1：

```text
输入：num = "132", change = [9,8,5,0,3,6,4,2,6,8]
输出："832"
解释：替换子字符串 "1"：
```

- 1 映射为 change[1] = 8 。

因此 "132" 变为 "832" 。
"832" 是可以构造的最大整数，所以返回它的字符串表示。

示例 2：

```text
输入：num = "021", change = [9,4,3,5,7,2,1,9,0,6]
输出："934"
解释：替换子字符串 "021"：
```

- 0 映射为 change[0] = 9 。
- 2 映射为 change[2] = 3 。
- 1 映射为 change[1] = 4 。

因此，"021" 变为 "934" 。
"934" 是可以构造的最大整数，所以返回它的字符串表示。

示例 3：

```text
输入：num = "5", change = [1,4,7,5,3,2,5,6,9,4]
输出："5"
解释："5" 已经是可以构造的最大整数，所以返回它的字符串表示。
```

__提示：__

- `1 <= num.length <= 10 ^ 5`
- `num` 仅由数字 `0-9` 组成
- `change.length == 10`
- `0 <= change[d] <= 9`

__思路:__

```text
贪心
从左到右遍历找到第一个可以映射增加的位置
从该位置开始向右遍历, 直到遇到第一个映射减少的位置
将该区间内的数字全部替换为映射后的数字
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string maximumNumber(string num, vector<int>& change) 
    {
        int i = 0, n = num.size();
        while (i < n and num[i] - '0' >= change[num[i] - '0']) ++i;
        for (; i < n and num[i] - '0' <= change[num[i] - '0']; i++)num[i] = change[num[i] - '0'] + '0';
        return num;
    }
};
```

__Java__:

```Java
class Solution {
    public String maximumNumber(String num, int[] change) {
        char[] c = num.toCharArray();
        for (int i = 0, n = c.length; i < n; i++) {
            if (change[c[i] - '0'] > c[i] - '0') {
                while (i < n && change[c[i] - '0'] >= c[i] - '0') c[i] = (char)(change[c[i++] - '0'] + '0');
                break;
            }
        }
        return new String(c);
    }
}
```

__Python__:

```Python
class Solution:
    def maximumNumber(self, num: str, change: List[int]) -> str:
        n, num = len(num), list(num)
        for i in range(n):
            if change[int(num[i])] > int(num[i]):
                while i < n and change[int(num[i])] >= int(num[i]):
                    num[i] = str(change[int(num[i])])
                    i += 1
                break
        return ''.join(num)
```
