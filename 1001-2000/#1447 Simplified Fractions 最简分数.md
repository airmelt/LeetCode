# 1447 Simplified Fractions 最简分数

__Description:__

Given an integer `n`, return _a list of all __simplified__ fractions between_ `0` _and_ `1` _(exclusive) such that the denominator is less-than-or-equal-to_ `n`. You can return the answer in __any order__.

__Example:__

Example 1:

```text
Input: n = 2
Output: ["1/2"]
Explanation: "1/2" is the only unique fraction with a denominator less-than-or-equal-to 2.
```

Example 2:

```text
Input: n = 3
Output: ["1/2","1/3","2/3"]
```

Example 3:

```text
Input: n = 4
Output: ["1/2","1/3","1/4","2/3","3/4"]
Explanation: "2/4" is not a simplified fraction because it can be simplified to "1/2".
```

__Constraints:__

- `1 <= n <= 100`

__题目描述:__

给你一个整数 `n` ，请你返回所有 0 到 1 之间（不包括 0 和 1）满足分母小于等于  `n` 的 __最简__ 分数 。分数可以以 __任意__ 顺序返回。

__示例:__

示例 1：

```text
输入：n = 2
输出：["1/2"]
解释："1/2" 是唯一一个分母小于等于 2 的最简分数。
```

示例 2：

```text
输入：n = 3
输出：["1/2","1/3","2/3"]
```

示例 3：

```text
输入：n = 4
输出：["1/2","1/3","1/4","2/3","3/4"]
解释："2/4" 不是最简分数，因为它可以化简为 "1/2" 。
```

示例 4：

```text
输入：n = 1
输出：[]
```

__提示：__

- `1 <= n <= 100`

__思路:__

```text
模拟
需要计算两个数是否有公因数
可以使用辗转相除法, 时间复杂度为 O(logN)
时间复杂度为 O(N ^ 2logN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<string> simplifiedFractions(int n) 
    {
        vector<string> result;
        for (int i = 2; i <= n; i++) for (int j = 1; j < i; j++) if (__gcd(i, j) == 1) result.emplace_back(to_string(j) + "/" + to_string(i));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> simplifiedFractions(int n) {
        List<String> result = new ArrayList<>();
        for (int i = 2; i <= n; i++) for (int j = 1; j < i; j++) if (gcd(i, j) == 1) result.add(j + "/" + i);
        return result;
    }
    
    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

__Python__:

```Python
class Solution:
    def simplifiedFractions(self, n: int) -> List[str]:
        return [f"{j}/{i}" for i in range(2, n + 1) for j in range(1, i) if gcd(i, j) == 1]
```
