# 2117 Abbreviating the Product of a Range 一个区间内所有数乘积的缩写

__Description:__

You are given two positive integers `left` and `right` with `left <= right`. Calculate the __product__ of all integers in the __inclusive__ range `[left, right]`.

Since the product may be very large, you will abbreviate it following these steps:

- For example, there are `3` trailing zeros in `1000`, and there are `0` trailing zeros in `546`.

- For example, we express `1234567654321` as `12345...54321`, but `1234567` is represented as `1234567`.

- For example, `12345678987600000` will be represented as `"12345...89876e5"`.

Return _a string denoting the __abbreviated product__ of all integers in the __inclusive__ range_ `[left, right]`.

__Example:__

Example 1:

```text
Input: left = 1, right = 4
Output: "24e0"
Explanation: The product is 1 × 2 × 3 × 4 = 24.
There are no trailing zeros, so 24 remains the same. The abbreviation will end with "e0".
Since the number of digits is 2, which is less than 10, we do not have to abbreviate it further.
Thus, the final representation is "24e0".
```

Example 2:

```text
Input: left = 2, right = 11
Output: "399168e2"
Explanation: The product is 39916800.
There are 2 trailing zeros, which we remove to get 399168. The abbreviation will end with "e2".
The number of digits after removing the trailing zeros is 6, so we do not abbreviate it further.
Hence, the abbreviated product is "399168e2".
```

Example 3:

```text
Input: left = 371, right = 375
Output: "7219856259e3"
Explanation: The product is 7219856259000.
```

__Constraints:__

- `1 <= left <= right <= 10 ^ 4`

__题目描述:__

给你两个正整数 `left` 和 `right` ，满足 `left <= right` 。请你计算 __闭区间__ `[left, right]` 中所有整数的 __乘积__ 。

由于乘积可能非常大，你需要将它按照以下步骤 缩写 ：

- 比方说， `1000` 中有 `3` 个后缀 0 ， `546` 中没有后缀 0 。

- 比方说，我们将 `1234567654321` 表示为 `12345...54321` ，但是 `1234567` 仍然表示为 `1234567` 。

- 比方说， `12345678987600000` 被表示为 `"12345...89876e5"` 。

请你返回一个字符串，表示 __闭区间__ `[left, right]` 中所有整数 __乘积__ 的 __缩写__ 。

__示例:__

示例 1：

```text
输入：left = 1, right = 4
输出："24e0"
解释：
乘积为 1 × 2 × 3 × 4 = 24 。
由于没有后缀 0 ，所以 24 保持不变，缩写的结尾为 "e0" 。
因为乘积的结果是 2 位数，小于 10 ，所欲我们不进一步将它缩写。
所以，最终将乘积表示为 "24e0" 。
```

示例 2：

```text
输入：left = 2, right = 11
输出："399168e2"
解释：乘积为 39916800 。
有 2 个后缀 0 ，删除后得到 399168 。缩写的结尾为 "e2" 。 
删除后缀 0 后是 6 位数，不需要进一步缩写。 
所以，最终将乘积表示为 "399168e2" 。
```

示例 3：

```text
输入：left = 371, right = 375
输出："7219856259e3"
解释：乘积为 7219856259000 。
```

__提示：__

- `1 <= left <= right <= 10 ^ 4`

__思路:__

```text
数学
本题可以转化为 3 个部分
计算有多少个 0 可以计算每个数有多少个 5 的因子
也可以通过保留前 12 位有效数字并对 10 取余并计数来得到
计算前 5 位数可以保留 12 位有效数字, 这里如果位数不足 10 位, 可以直接返回计算结果
计算后 5 位数可以用 double 类型计算, 保留 1 位整数即可, 同时可以计算舍去的位数 c
若 c - zero < 10, 则返回前 5 位数 + 后 5 位数 + "e" + zero, 即不需要省略
否则返回前 5 位数 + "..." + 后 5 位数 + "e" + zero, 即需要省略
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string abbreviateProduct(int left, int right) 
    {
        long long zero = 0LL, suf = 1LL, mod = 1e10, c = 0LL, flag = true;
        for (int i = left; i < right + 1; i++) 
        {
            suf *= i;
            while (!(suf % 10)) 
            {
                zero++;
                suf /= 10;
            }
            if (suf > mod) 
            {
                flag = false;
                suf %= mod;
            }
        }
        if (flag) return to_string(suf) + "e" + to_string(zero);
        double pre = 1.0;
        for (int i = left; i < right + 1; i++) 
        {
            pre *= i;
            while (pre > 10) 
            {
                pre /= 10;
                ++c;
            }
        }
        pre *= 1000000;
        string x = to_string(pre), y = to_string(suf);
        while (y.size() < 5) y = "0" + y;
        return c - zero < 10 ? x.substr(0, 5) + y.substr(y.size() - 5, 5) + "e" + to_string(zero) : x.substr(0, 5) + "..." + y.substr(y.size() - 5, 5) + "e" + to_string(zero);
    }
};
```

__Java__:

```Java
class Solution {
    public String abbreviateProduct(int left, int right) {
        long zero = 0L, suf = 1L, mod = 10_000_000_000L, count = 0;
        boolean flag = true;
        for (int i = left; i < right + 1; i++) {
            suf *= i;
            while (suf % 10 == 0) {
                zero++;
                suf /= 10;
            }
            if (suf > mod) {
                flag = false;
                suf %= mod;
            }
        }
        if (flag) return suf + "e" + zero;
        double pre = 1.0;
        for (int i = left; i < right + 1; i++) {
            pre *= i;
            while (pre > 10) {
                pre /= 10;
                ++count;
            }
        }
        String x = String.valueOf(pre), y = String.valueOf(suf);
        x = x.replace(".", "");
        while (y.length() < 5) y = "0" + y;
        return count - zero < 10 ? x.substring(0, 5) + y.substring(y.length() - 5) + "e" + zero : x.substring(0, 5) + "..." + y.substring(y.length() - 5) + "e" + zero;
    }
}
```

__Python__:

```Python
class Solution:
    def abbreviateProduct(self, left: int, right: int) -> str:
        sys.set_int_max_str_digits(100000)
        return s + 'e' + str(len(str(result)) - len(s)) if len(s := str(result := reduce(lambda x, y: x * y, range(left, right + 1))).rstrip('0')) < 11 else s[:5] + '...' + s[-5:] + 'e' + str(len(str(result)) - len(s))
```
