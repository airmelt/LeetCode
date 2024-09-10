# 2243 Calculate Digit Sum of a String 计算字符串的数字和

__Description:__

You are given a string `s` consisting of digits and an integer `k`.

A __round__ can be completed if the length of `s` is greater than `k`. In one round, do the following:

Return `s` _after all rounds have been completed_.

__Example:__

Example 1:

```text
Input: s = "11111222223", k = 3
Output: "135"
Explanation: 
```

- For the first round, we divide s into groups of size 3: "111", "112", "222", and "23".
  Then we calculate the digit sum of each group: 1 + 1 + 1 = 3, 1 + 1 + 2 = 4, 2 + 2 + 2 = 6, and 2 + 3 = 5.
  So, s becomes "3" + "4" + "6" + "5" = "3465" after the first round.
- For the second round, we divide s into "346" and "5".
  Then we calculate the digit sum of each group: 3 + 4 + 6 = 13, 5 = 5.
  So, s becomes "13" + "5" = "135" after second round.

Now, s.length <= k, so we return "135" as the answer.

Example 2:

```text
Input: s = "00000000", k = 3
Output: "000"
Explanation: 
We divide s into "000", "000", and "00".
Then we calculate the digit sum of each group: 0 + 0 + 0 = 0, 0 + 0 + 0 = 0, and 0 + 0 = 0. 
s becomes "0" + "0" + "0" = "000", whose length is equal to k, so we return "000".
```

__Constraints:__

- `1 <= s.length <= 100`
- `2 <= k <= 100`
- `s` consists of digits only.

__题目描述:__

给你一个由若干数字（ `0` - `9`）组成的字符串 `s` ，和一个整数。

如果 `s` 的长度大于 `k` ，则可以执行一轮操作。在一轮操作中，需要完成以下工作:

返回在完成所有轮操作后的 `s` 。

__示例:__

示例 1：

```text
输入：s = "11111222223", k = 3
输出："135"
解释：
```

- 第一轮，将 s 分成："111"、"112"、"222" 和 "23" 。
  接着，计算每一组的数字和：1 + 1 + 1 = 3、1 + 1 + 2 = 4、2 + 2 + 2 = 6 和 2 + 3 = 5 。
  这样，s 在第一轮之后变成 "3" + "4" + "6" + "5" = "3465" 。
- 第二轮，将 s 分成："346" 和 "5" 。
  接着，计算每一组的数字和：3 + 4 + 6 = 13 、5 = 5 。
  这样，s 在第二轮之后变成 "13" + "5" = "135" 。
  
现在，s.length <= k ，所以返回 "135" 作为答案。

示例 2：

```text
输入：s = "00000000", k = 3
输出："000"
解释：
将 "000", "000", and "00".
接着，计算每一组的数字和：0 + 0 + 0 = 0 、0 + 0 + 0 = 0 和 0 + 0 = 0 。 
s 变为 "0" + "0" + "0" = "000" ，其长度等于 k ，所以返回 "000" 。
```

__提示：__

- `1 <= s.length <= 100`
- `2 <= k <= 100`
- `s` 仅由数字（ `0` - `9`）组成。

__思路:__

```text
递归
简单模拟
将 s 按照 k 分组, 计算每一组的数字和
递归调用, 直到 s 的长度小于等于 k
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string digitSum(string s, int k) 
    {
        int n = s.size();
        if (n <= k) return s;
        string next = "";
        for (int i = 0; i < n; i += k) 
        {
            int cur = 0;
            for (int j = 0; j < k and i + j < n; j++) cur += (s[i + j] & 15);
            next += to_string(cur);
        }
        return digitSum(next, k); 
    }
};
```

__Java__:

```Java
class Solution {
    public String digitSum(String s, int k) {
        int n = s.length();
        if (n <= k) return s;
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n; i += k) {
            int cur = 0;
            for (int j = 0; j < k && i + j < n; j++) cur += (s.charAt(i + j) & 15);
            sb.append(cur);
        }
        return digitSum(sb.toString(), k);
    }
}
```

__Python__:

```Python
class Solution:
    def digitSum(self, s: str, k: int) -> str:
        return s if len(s) <= k else self.digitSum("".join([str(sum(int(i) for i in s[j:j + k])) for j in range(0, len(s), k)]), k)
```
