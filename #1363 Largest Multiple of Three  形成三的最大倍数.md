# 1363 Largest Multiple of Three  形成三的最大倍数

__Description:__

Given an array of digits digits, return the largest multiple of three that can be formed by concatenating some of the given digits in any order. If there is no answer return an empty string.

Since the answer may not fit in an integer data type, return the answer as a string. Note that the returning answer must not contain unnecessary leading zeros.

__Example:__

Example 1:

Input: digits = [8,1,9]
Output: "981"

Example 2:

Input: digits = [8,6,7,1,0]
Output: "8760"

Example 3:

Input: digits = [1]
Output: ""

__Constraints:__

1 <= digits.length <= 10^4
0 <= digits[i] <= 9

__题目描述：__

给你一个整数数组 digits，你可以通过按任意顺序连接其中某些数字来形成 3 的倍数，请你返回所能得到的最大的 3 的倍数。

由于答案可能不在整数数据类型范围内，请以字符串形式返回答案。

如果无法得到答案，请返回一个空字符串。

__示例：__

示例 1：

输入：digits = [8,1,9]
输出："981"

示例 2：

输入：digits = [8,6,7,1,0]
输出："8760"

示例 3：

输入：digits = [1]
输出：""

示例 4：

输入：digits = [0,0,0,0,0,0]
输出："0"

__提示：__

1 <= digits.length <= 10^4
0 <= digits[i] <= 9
返回的结果不应包含不必要的前导零。

__思路：__

贪心
统计所有数字出现的次数和所有数字之和 s
如果 s 能整除 3, 从大到小输出所有数字即可
如果 s 除 3 余 1, 要么去掉一个最小的除 3 余 1 的数, 要么去掉 2 个最小除 3 余 2 的数
如果 s 除 3 余 2, 要么去掉一个最小的除 3 余 2 的数, 要么去掉 2 个最小除 3 余 2 的数
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
private:
    int helper(int m, vector<int>& count)
    {
        for (int i = m; i <= 9; i += 3) if (count[i])
        {
            count[i]--;
            return 1;
        }
        return 0;
    }
public:
    string largestMultipleOfThree(vector<int>& digits) 
    {
        string result = "";
        vector<int> count(10);
        int s = accumulate(digits.begin(), digits.end(), 0);
        for (const auto& x : digits) ++count[x];
        if (s % 3 == 1 and !helper(1, count))
        {
            helper(2, count);
            helper(2, count);
        }
        if (s % 3 == 2 and !helper(2, count))
        {
            helper(1, count);
            helper(1, count);
        }
        for (int i = 9; i > -1; i--) while(count[i]--) result += i + '0';
        return result.size() and result.front() == '0' ? "0" : result;
    }
};
```

__Java__:

```Java
class Solution {
    public String largestMultipleOfThree(int[] digits) {
        StringBuilder sb = new StringBuilder();
        int s = 0, count[] = new int[10];
        for (int x : digits) {
            ++count[x];
            s += x;
        }
        if (s % 3 == 1 && !helper(1, count)) {
            helper(2, count);
            helper(2, count);
        }
        if (s % 3 == 2 && !helper(2, count)) {
            helper(1, count);
            helper(1, count);
        }
        for (int i = 9; i > -1; i--) while (count[i]-- != 0) sb.append(i);
        return sb.length() > 0 && sb.charAt(0) == '0' ? "0" : sb.toString();
    }
    
    private boolean helper(int m, int[] count) {
        for (int i = m; i <= 9; i += 3) if (count[i] != 0) {
            --count[i];
            return true;
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def largestMultipleOfThree(self, digits: List[int]) -> str:
        result, s, count = '', sum(digits), Counter(digits)
        
        def helper(m: int) -> bool:
            for i in range(m, 10, 3):
                if count[i]:
                    count[i] -= 1
                    return True
            return False
        if s % 3 == 1 and not helper(1):
            helper(2)
            helper(2)
        if s % 3 == 2 and not helper(2):
            helper(1)
            helper(1)
        for i in range(9, -1, -1):
            while count[i]:
                result += str(i)
                count[i] -= 1
        return '0' if result and result[0] == '0' else result
```
