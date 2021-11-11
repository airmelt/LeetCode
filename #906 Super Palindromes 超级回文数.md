# 906 Super Palindromes 超级回文数

__Description__:
Let's say a positive integer is a super-palindrome if it is a palindrome, and it is also the square of a palindrome.

Given two positive integers left and right represented as strings, return the number of super-palindromes integers in the inclusive range [left, right].

__Example:__

Example 1:

Input: left = "4", right = "1000"
Output: 4
Explanation: 4, 9, 121, and 484 are superpalindromes.
Note that 676 is not a superpalindrome: 26 * 26 = 676, but 26 is not a palindrome.

Example 2:

Input: left = "1", right = "2"
Output: 1

__Constraints:__

1 <= left.length, right.length <= 18
left and right consist of only digits.
left and right cannot have leading zeros.
left and right represent integers in the range [1, 10^18 - 1].
left is less than or equal to right.

__题目描述__:
如果一个正整数自身是回文数，而且它也是一个回文数的平方，那么我们称这个数为超级回文数。

现在，给定两个正整数 L 和 R （以字符串形式表示），返回包含在范围 [L, R] 中的超级回文数的数目。

__示例 :__

输入：L = "4", R = "1000"
输出：4
解释：
4，9，121，以及 484 是超级回文数。
注意 676 不是一个超级回文数： 26 * 26 = 676，但是 26 不是回文数。

__提示:__

1 <= len(L) <= 18
1 <= len(R) <= 18
L 和 R 是表示 [1, 10^18) 范围的整数的字符串。
int(L) <= int(R)

__思路__:

1. 打表
明显没几个超级回文数, 预处理之后打表
时间复杂度为 O(right), 空间复杂度为 O(1)
2. 构造法
假定一个数 num 是超级回文数, 那么范围为 0-10 ^ 18
则开方之后 k = num ^ 1 / 2, 最大为 10 ^ 9, k 也为回文数
可以将 k 看作前后两个部分, k1 & k2, len(k1) == len(k2), k1 == k2[::-1] 或者 len(k1) == len(k2) + 1, k1[:-1] == k2[::-1]
k1, k2 < 10 ^ 5, 那么从 0 构造到 10 ^ 5 分别检查是否为超级回文数即可
时间复杂度为 O(1), 空间复杂度为 O(1), 只需要检查常数个数字

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int superpalindromesInRange(string left, string right) 
    {
        vector<long> nums{ 0, 1, 4, 9, 121, 484, 10201, 12321, 14641, 40804, 44944, 1002001, 1234321, 4008004, 100020001, 102030201, 104060401, 121242121, 123454321, 125686521, 400080004, 404090404, 10000200001, 10221412201, 12102420121, 12345654321, 40000800004, 1000002000001, 1002003002001, 1004006004001, 1020304030201, 1022325232201, 1024348434201, 1210024200121, 1212225222121, 1214428244121, 1232346432321, 1234567654321, 4000008000004, 4004009004004, 100000020000001, 100220141022001, 102012040210201, 102234363432201, 121000242000121, 121242363242121, 123212464212321, 123456787654321, 400000080000004, 10000000200000001, 10002000300020001, 10004000600040001, 10020210401202001, 10022212521222001, 10024214841242001, 10201020402010201, 10203040504030201, 10205060806050201, 10221432623412201, 10223454745432201, 12100002420000121, 12102202520220121, 12104402820440121, 12122232623222121, 12124434743442121, 12321024642012321, 12323244744232321, 12343456865434321, 12345678987654321, 40000000800000004, 40004000900040004 };
        long result = 0, l = stol(left), r = stol(right);
        for (const auto& num : nums) if (l <= num and num <= r) ++result;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int superpalindromesInRange(String left, String right) {
        int result = 0;
        for (int a = Integer.parseInt("1" + "0".repeat(String.valueOf((int)Math.sqrt(Double.parseDouble(left))).length() - 1), 3); ; a++) {
            String check = Integer.toString(a, 3);
            if (!new StringBuffer(check).reverse().toString().equals(check) || Integer.parseInt(check) < (int)Math.sqrt(Double.parseDouble(left))) continue;
            if (Integer.parseInt(check) > (int)Math.sqrt(Double.parseDouble(right))) break;
            if (check.chars().map(b -> ((char)b - '0') * ((char)b - '0')).sum() < 10) ++result;
        }
        if(left.length() == 1 && Integer.parseInt(left) <= 9 && (right.length() > 1 || Integer.parseInt(right) >= 9)) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def superpalindromesInRange(self, left: str, right: str) -> int:
        return bisect_left((s := [0, 1, 4, 9, 121, 484, 10201, 12321, 14641, 40804, 44944, 1002001, 1234321, 4008004, 100020001, 102030201, 104060401, 121242121, 123454321, 125686521, 400080004, 404090404, 10000200001, 10221412201, 12102420121, 12345654321, 40000800004, 1000002000001, 1002003002001, 1004006004001, 1020304030201, 1022325232201, 1024348434201, 1210024200121, 1212225222121, 1214428244121, 1232346432321, 1234567654321, 4000008000004, 4004009004004, 100000020000001, 100220141022001, 102012040210201, 102234363432201, 121000242000121, 121242363242121, 123212464212321, 123456787654321, 400000080000004, 10000000200000001, 10002000300020001, 10004000600040001, 10020210401202001, 10022212521222001, 10024214841242001, 10201020402010201, 10203040504030201, 10205060806050201, 10221432623412201, 10223454745432201, 12100002420000121, 12102202520220121, 12104402820440121, 12122232623222121, 12124434743442121, 12321024642012321, 12323244744232321, 12343456865434321, 12345678987654321, 40000000800000004, 40004000900040004]), int(right)) - bisect_left(s, int(left))
```
