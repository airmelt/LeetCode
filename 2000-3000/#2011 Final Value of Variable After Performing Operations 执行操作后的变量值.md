# 2011 Final Value of Variable After Performing Operations 执行操作后的变量值

__Description:__

There is a programming language with only __four__ operations and __one__ variable `X`:

- `++X` and `X++` __increments__ the value of the variable `X` by `1`.
- `--X` and `X--` __decrements__ the value of the variable `X` by `1`.

Initially, the value of `X` is `0`.

Given an array of strings `operations` containing a list of operations, return _the __final__ value of_ `X` _after performing all the operations_.

__Example:__

Example 1:

```text
Input: operations = ["--X","X++","X++"]
Output: 1
Explanation: The operations are performed as follows:
Initially, X = 0.
--X: X is decremented by 1, X =  0 - 1 = -1.
X++: X is incremented by 1, X = -1 + 1 =  0.
X++: X is incremented by 1, X =  0 + 1 =  1.
```

Example 2:

```text
Input: operations = ["++X","++X","X++"]
Output: 3
Explanation: The operations are performed as follows:
Initially, X = 0.
++X: X is incremented by 1, X = 0 + 1 = 1.
++X: X is incremented by 1, X = 1 + 1 = 2.
X++: X is incremented by 1, X = 2 + 1 = 3.
```

Example 3:

```text
Input: operations = ["X++","++X","--X","X--"]
Output: 0
Explanation: The operations are performed as follows:
Initially, X = 0.
X++: X is incremented by 1, X = 0 + 1 = 1.
++X: X is incremented by 1, X = 1 + 1 = 2.
--X: X is decremented by 1, X = 2 - 1 = 1.
X--: X is decremented by 1, X = 1 - 1 = 0.
```

__Constraints:__

- `1 <= operations.length <= 100`
- `operations[i]` will be either `"++X"`, `"X++"`, `"--X"`, or `"X--"`.

__题目描述:__

存在一种仅支持 4 种操作和 1 个变量 `X` 的编程语言:

- `++X` 和 `X++` 使变量 `X` 的值 __加__ `1`
- `--X` 和 `X--` 使变量 `X` 的值 __减__ `1`

最初， `X` 的值是 `0`

给你一个字符串数组 `operations` ，这是由操作组成的一个列表，返回执行所有操作后， `X` 的 __最终值__ 。

__示例:__

示例 1：

```text
输入：operations = ["--X","X++","X++"]
输出：1
解释：操作按下述步骤执行：
最初，X = 0
--X：X 减 1 ，X =  0 - 1 = -1
X++：X 加 1 ，X = -1 + 1 =  0
X++：X 加 1 ，X =  0 + 1 =  1
```

示例 2：

```text
输入：operations = ["++X","++X","X++"]
输出：3
解释：操作按下述步骤执行： 
最初，X = 0
++X：X 加 1 ，X = 0 + 1 = 1
++X：X 加 1 ，X = 1 + 1 = 2
X++：X 加 1 ，X = 2 + 1 = 3
```

示例 3：

```text
输入：operations = ["X++","++X","--X","X--"]
输出：0
解释：操作按下述步骤执行：
最初，X = 0
X++：X 加 1 ，X = 0 + 1 = 1
++X：X 加 1 ，X = 1 + 1 = 2
--X：X 减 1 ，X = 2 - 1 = 1
X--：X 减 1 ，X = 1 - 1 = 0
```

__提示：__

- `1 <= operations.length <= 100`
- `operations[i]` 将会是 `"++X"`、 `"X++"`、 `"--X"` 或 `"X--"`

__思路:__

```text
模拟
注意到加和减只有第 1 位不同
遍历 operations 数组，如果第 1 位为 '+' 则加 1，否则减 1
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int finalValueAfterOperations(vector<string>& operations) 
    {
        int result = 0;
        for (const auto& s : operations) result += (s[1] == '+' ? 1 : -1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int finalValueAfterOperations(String[] operations) {
        int result = 0;
        for (String s : operations) result += (s.charAt(1) == '+' ? 1 : -1);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def finalValueAfterOperations(self, operations: List[str]) -> int:
        return sum(1 if s[1] == '+' else -1 for s in operations) 
```
