# 1432 Max Difference You Can Get From Changing an Integer 改变一个整数能得到的最大差值

__Description:__

You are given an integer `num`. You will apply the following steps exactly __two__ times:

- Pick a digit `x (0 <= x <= 9)`.
- Pick another digit `y (0 <= y <= 9)`. The digit `y` can be equal to `x`.
- Replace all the occurrences of `x` in the decimal representation of `num` by `y`.
- The new integer __cannot__ have any leading zeros, also the new integer __cannot__ be 0.

Let `a` and `b` be the results of applying the operations to `num` the first and second times, respectively.

Return _the max difference_ between `a` and `b`.

__Example:__

Example 1:

```text
Input: num = 555
Output: 888
Explanation: The first time pick x = 5 and y = 9 and store the new integer in a.
The second time pick x = 5 and y = 1 and store the new integer in b.
We have now a = 999 and b = 111 and max difference = 888
```

Example 2:

```text
Input: num = 9
Output: 8
Explanation: The first time pick x = 9 and y = 9 and store the new integer in a.
The second time pick x = 9 and y = 1 and store the new integer in b.
We have now a = 9 and b = 1 and max difference = 8
```

__Constraints:__

- `1 <= num <= 10 ^ 8`

__题目描述:__

给你一个整数 `num` 。你可以对它进行如下步骤恰好 __两次__ ：

- 选择一个数字 `x (0 <= x <= 9)`.
- 选择另一个数字 `y (0 <= y <= 9)` 。数字 `y` 可以等于 `x` 。
- 将 `num` 中所有出现 `x` 的数位都用 `y` 替换。
- 得到的新的整数 __不能__ 有前导 0 ，得到的新整数也 __不能__ 是 0 。

令两次对 `num` 的操作得到的结果分别为 `a` 和 `b` 。

请你返回 `a` 和 `b` 的 __最大差值__ 。

__示例:__

示例 1：

```text
输入：num = 555
输出：888
解释：第一次选择 x = 5 且 y = 9 ，并把得到的新数字保存在 a 中。
第二次选择 x = 5 且 y = 1 ，并把得到的新数字保存在 b 中。
现在，我们有 a = 999 和 b = 111 ，最大差值为 888
```

示例 2：

```text
输入：num = 9
输出：8
解释：第一次选择 x = 9 且 y = 9 ，并把得到的新数字保存在 a 中。
第二次选择 x = 9 且 y = 1 ，并把得到的新数字保存在 b 中。
现在，我们有 a = 9 和 b = 1 ，最大差值为 8
```

示例 3：

```text
输入：num = 123456
输出：820000
```

示例 4：

```text
输入：num = 10000
输出：80000
```

示例 5：

```text
输入：num = 9288
输出：8700
```

__提示：__

- `1 <= num <= 10 ^ 8`

__思路:__

```text
贪心
最大的数为找到 num 第一个不为 9 的数字全部替换为 9
最小的数为找到 num 第一个不为 0 的数字全部替换为 0(不能是前导 0)
或者将最高位替换成 1
时间复杂度为 O(logN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    void replace(string& s, char x, char y) 
    {
        for (int i = 0, n = s.size(); i < n; i++) if (s[i] == x) s[i] = y;
    }
public:
    int maxDiff(int num) 
    {
        string small = to_string(num), big = to_string(num);
        int n = small.size();
        for (int i = 0; i < n; i++) 
        {
            char c = big[i];
            if (c != '9') 
            {
                replace(big, c, '9');
                break;
            }
        }
        for (int i = 0; i < n; i++) 
        {
            char c = small[i];
            if (i == 0) 
            {
                if (c != '1')
                {
                    replace(small, c, '1');
                    break;
                }
            }
            else 
            {
                if (c != '0' and c != small.front()) 
                {
                    replace(small, c, '0');
                    break;
                }
            }
        }
        return stoi(big) - stoi(small);
    }
};
```

__Java__:

```Java
class Solution {
    public int maxDiff(int num) {
        StringBuilder small = new StringBuilder(String.valueOf(num)), big = new StringBuilder(String.valueOf(num));
        int n = small.length();
        for (int i = 0; i < n; i++) {
            char c = big.charAt(i);
            if (c != '9') {
                replace(big, c, '9');
                break;
            }
        }
        for (int i = 0; i < n; i++) {
            char c = small.charAt(i);
            if (i == 0) {
                if (c != '1') {
                    replace(small, c, '1');
                    break;
                }
            }
            else {
                if (c != '0' && c != small.charAt(0)) {
                    replace(small, c, '0');
                    break;
                }
            }
        }
        return Integer.parseInt(big.toString()) - Integer.parseInt(small.toString());
    }
    
    private void replace(StringBuilder s, char x, char y) {
        for (int i = 0, n = s.length(); i < n; i++) if (s.charAt(i) == x) s.setCharAt(i, y);
    }
}
```

__Python__:

```Python
class Solution:
    def maxDiff(self, num: int) -> int:
        return max(cur := list(int(p) for i in range(10) for j in range(10) if (p := str(num).replace(str(i), str(j)))[0] != '0' and int(p))) - min(cur)
```
