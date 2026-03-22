# 869 Reordered Power of 2 重新排序得到 2 的幂

__Description__:
You are given an integer n. We reorder the digits in any order (including the original order) such that the leading digit is not zero.

Return true if and only if we can do this so that the resulting number is a power of two.

__Example:__

Example 1:

Input: n = 1
Output: true

Example 2:

Input: n = 10
Output: false

Example 3:

Input: n = 16
Output: true

Example 4:

Input: n = 24
Output: false

Example 5:

Input: n = 46
Output: true

__Constraints:__

1 <= n <= 10^9

__题目描述__:
给定正整数 N ，我们按任何顺序（包括原始顺序）将数字重新排序，注意其前导数字不能为零。

如果我们可以通过上述方式得到 2 的幂，返回 true；否则，返回 false。

__示例 :__

示例 1：

输入：1
输出：true

示例 2：

输入：10
输出：false

示例 3：

输入：16
输出：true

示例 4：

输入：24
输出：false

示例 5：

输入：46
输出：true

__提示:__

1 <= N <= 10^9

__思路__:

计数排序
将输入的每一位记录出现次数
与 2 的 n 次方每一位的数字记录出现次数比较
只要有一个相等就返回 true
时间复杂度为 O(lg^2n), 空间复杂度为 O(lgn)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool reorderedPowerOf2(int n) 
    {
        vector<int> a = count(n), b;
        for (int i = 0; i < 31; i++) if (equal(a, (b = count(1 << i)))) return true;
        return false;
    }
private:
    vector<int> count(int n) 
    {
        vector<int> result(10, 0);
        while (n) 
        {
            ++result[n % 10];
            n /= 10;
        }
        return result;
    }
    
    bool equal(vector<int> &a, vector<int> &b)
    {
        if (a.size() != b.size()) return false;
        for (int i = 0; i < a.size(); i++) if (a[i] != b[i]) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean reorderedPowerOf2(int n) {
        String[] powers = { "1", "2", "4", "8", "16", "23", "46", "128", "256", "125", "0124", "0248", "0469", "1289", "13468",  "23678", "35566", "011237", "122446", "224588", "0145678", "0122579", "0134449", "0368888",  "11266777", "23334455", "01466788", "112234778", "234455668", "012356789", "0112344778" };
        char[] nums = String.valueOf(n).toCharArray();
        Arrays.sort(nums);
        String str = new String(nums);
        for (String p : powers) if (str.equals(p)) return true;
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def reorderedPowerOf2(self, n: int) -> bool:
        return sorted(list(str(n))) in (sorted(list(str(2 ** i))) for i in range(30))
```
