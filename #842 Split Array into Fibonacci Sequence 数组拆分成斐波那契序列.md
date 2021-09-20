# 842 Split Array into Fibonacci Sequence 数组拆分成斐波那契序列

__Description__:
You are given a string of digits num, such as "123456579". We can split it into a Fibonacci-like sequence [123, 456, 579].

Formally, a Fibonacci-like sequence is a list f of non-negative integers such that:

0 <= f[i] < 2^31, (that is, each integer fits in a 32-bit signed integer type),
f.length >= 3, and
f[i] + f[i + 1] == f[i + 2] for all 0 <= i < f.length - 2.
Note that when splitting the string into pieces, each piece must not have extra leading zeroes, except if the piece is the number 0 itself.

Return any Fibonacci-like sequence split from num, or return [] if it cannot be done.

__Example:__

Example 1:

Input: num = "123456579"
Output: [123,456,579]

Example 2:

Input: num = "11235813"
Output: [1,1,2,3,5,8,13]

Example 3:

Input: num = "112358130"
Output: []
Explanation: The task is impossible.

Example 4:

Input: num = "0123"
Output: []
Explanation: Leading zeroes are not allowed, so "01", "2", "3" is not valid.

Example 5:

Input: num = "1101111"
Output: [11,0,11,11]
Explanation: The output [11, 0, 11, 11] would also be accepted.

__Constraints:__

1 <= num.length <= 200
num contains only digits.

__题目描述__:
给定一个数字字符串 S，比如 S = "123456579"，我们可以将它分成斐波那契式的序列 [123, 456, 579]。

形式上，斐波那契式序列是一个非负整数列表 F，且满足：

0 <= F[i] <= 2^31 - 1，（也就是说，每个整数都符合 32 位有符号整数类型）；
F.length >= 3；
对于所有的0 <= i < F.length - 2，都有 F[i] + F[i+1] = F[i+2] 成立。
另外，请注意，将字符串拆分成小块时，每个块的数字一定不要以零开头，除非这个块是数字 0 本身。

返回从 S 拆分出来的任意一组斐波那契式的序列块，如果不能拆分则返回 []。

__示例 :__

示例 1：

输入："123456579"
输出：[123,456,579]

示例 2：

输入: "11235813"
输出: [1,1,2,3,5,8,13]

示例 3：

输入: "112358130"
输出: []
解释: 这项任务无法完成。

示例 4：

输入："0123"
输出：[]
解释：每个块的数字不能以零开头，因此 "01"，"2"，"3" 不是有效答案。

示例 5：

输入: "1101111"
输出: [110, 1, 111]
解释: 输出 [11,0,11,11] 也同样被接受。

__提示:__

1 <= S.length <= 200
字符串 S 中只含有数字。

__思路__:

回溯法
从每一个位置开始查找是否能够分成斐波那契数列
如果找到前导 0 就直接中断循环
走到终点时, 结果数组需要有 3 个以上的数
找到大于 INT_MAX 当前的数大于前两个数之和的也直接中断循环
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> splitIntoFibonacci(string num) 
    {
        vector<int> result;
        backtrack(result, num.size(), 0, num);
        return result;
    }
private:
    bool backtrack(vector<int>& result, int n, int pos, string& num) 
    {
        int size = result.size();
        if (pos == n) return size > 2;
        long cur = 0;
        for (int i = pos; i < n; i++) 
        {
            if (i > pos and num[pos] == '0') break;
            cur = 10 * cur + num[i] - '0';
            if (cur > INT_MAX) break;
            if (size < 2 or cur == (long)result[size - 2] + (long)result.back()) 
            {
                result.emplace_back((int)cur);
                if (backtrack(result, n, i + 1, num)) return true;
                result.pop_back();
            }
            else if (size > 2 and cur > (long)result[size - 2] + (long)result.back()) break;
        }
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> splitIntoFibonacci(String num) {
        List<Integer> result = new ArrayList<>();
        int n = num.length();
        backtrack(result, n, 0, num);
        return result;
    }
    
    private boolean backtrack(List<Integer> result, int n, int pos, String num) {
        if (pos == n) return result.size() > 2;
        long cur = 0;
        for (int i = pos; i < n; i++) {
            if (i > pos && num.charAt(pos) == '0') break;
            cur = 10 * cur + num.charAt(i) - '0';
            if (cur > Integer.MAX_VALUE) break;
            if (result.size() < 2 || cur == result.get(result.size() - 2) + result.get(result.size() - 1)) {
                result.add((int)cur);
                if (backtrack(result, n, i + 1, num)) return true;
                result.remove(result.size() - 1);
            }
            else if (result.size() > 2 && cur > result.get(result.size() - 2) + result.get(result.size() - 1)) break;
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def splitIntoFibonacci(self, num: str) -> List[int]:
        result, n = [], len(num)

        def backtrack(pos: int) -> bool:
            if pos == n:
                return len(result) > 2
            cur = 0
            for i in range(pos, n):
                if i > pos and num[pos] == '0':
                    break
                cur = 10 * cur + ord(num[i]) - ord('0')
                if cur > (1 << 31) - 1:
                    break
                if len(result) < 2 or cur == result[-2] + result[-1]:
                    result.append(cur)
                    if backtrack(i + 1):
                        return True
                    result.pop()
                elif len(result) > 2 and cur > result[-2] + result[-1]:
                    break
            return False
        backtrack(0)
        return result
```
