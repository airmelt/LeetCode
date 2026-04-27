# 1702 Maximum Binary String After Change 修改后的最大二进制字符串

__Description:__

You are given a binary string `binary` consisting of only `0`'s or `1`'s. You can apply each of the following operations any number of times:

- Operation 1: If the number contains the substring `"00"`, you can replace it with `"10"`.
  - For example, `"00010" -> "10010`"
- Operation 2: If the number contains the substring `"10"`, you can replace it with `"01"`.
  - For example, `"00010" -> "00001"`

_Return the __maximum binary string__ you can obtain after any number of operations. Binary string `x` is greater than binary string `y` if `x`'s decimal representation is greater than `y`'s decimal representation._

__Example:__

Example 1:

```text
Input: binary = "000110"
Output: "111011"
Explanation: A valid transformation sequence can be:
"000110" -> "000101" 
"000101" -> "100101" 
"100101" -> "110101" 
"110101" -> "110011" 
"110011" -> "111011"
```

Example 2:

```text
Input: binary = "01"
Output: "01"
Explanation: "01" cannot be transformed any further.
```

__Constraints:__

- `1 <= binary.length <= 10 ^ 5`
- `binary` consist of `'0'` and `'1'`.

__题目描述:__

给你一个二进制字符串 `binary` ，它仅有 `0` 或者 `1` 组成。你可以使用下面的操作任意次对它进行修改:

- 操作 1 :如果二进制串包含子字符串 `"00"` ，你可以用 `"10"` 将其替换。
  - 比方说， `"00010" -> "10010"`
- 操作 2 :如果二进制串包含子字符串 `"10"` ，你可以用 `"01"` 将其替换。
  - 比方说， `"00010" -> "00001"`

请你返回执行上述操作任意次以后能得到的 __最大二进制字符串__ 。如果二进制字符串 `x` 对应的十进制数字大于二进制字符串 `y` 对应的十进制数字，那么我们称二进制字符串 `x` 大于二进制字符串 `y` 。

__示例:__

示例 1：

```text
输入：binary = "000110"
输出："111011"
解释：一个可行的转换为：
"000110" -> "000101" 
"000101" -> "100101" 
"100101" -> "110101" 
"110101" -> "110011" 
"110011" -> "111011"
```

示例 2：

```text
输入：binary = "01"
输出："01"
解释："01" 没办法进行任何转换。
```

__提示：__

- `1 <= binary.length <= 10 ^ 5`
- `binary` 仅包含 `'0'` 和 `'1'` 。

__思路:__

```text
数学
注意到只能修改 '00' 和 '10' 两种情况
所以开头的 '11' 和 '01' 都会得到保留
我们可以从左往右遍历得到第一次出现 '0' 的位置 left
将所有的 '1' 先移动到最右边
然后将左边的 '0' 都换成 '1'
比如 '000110' -> '000011' -> '111011'
所以我们需要得到 '0' 的个数 zero
如果 zero <= 1, 那么就不需要进行任何操作
否则需要返回 '1' * (left + zero - 1) + '0' + '1' * (n - left - zero), 即开头的 '1' + 中间的 '0' + 结尾的 '1', 中间有且仅有 1 个 ‘0’
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string maximumBinaryString(string binary) 
    {
        int zero = 0, left = binary.find('0'), n = binary.size();
        for (int i = 0; i < n; i++) zero += binary[i] == '0';
        return zero <= 1 ? binary : string(left + zero - 1, '1') + "0" + string(n - left - zero, '1');
    }
};
```

__Java__:

```Java
class Solution {
    public String maximumBinaryString(String binary) {
        int zero = 0, left = binary.indexOf('0'), n = binary.length();
        for (int i = 0; i < n; i++) if (binary.charAt(i) == '0') ++zero;
        return zero <= 1 ? binary : "1".repeat(left + zero - 1) + "0" + "1".repeat(n - left - zero);
    }
}
```

__Python__:

```Python
class Solution:
    def maximumBinaryString(self, binary: str) -> str:
        return binary if ((c1 := binary.count('1') - binary.find('0')) > (n := len(binary))) else (n - c1 - 1) * '1' + '0' + c1 * '1'
```
