# 899 Orderly Queue 有序队列

__Description__:
You are given a string s and an integer k. You can choose one of the first k letters of s and append it at the end of the string..

Return the lexicographically smallest string you could have after applying the mentioned step any number of moves.

__Example:__

Example 1:

Input: s = "cba", k = 1
Output: "acb"
Explanation:
In the first move, we move the 1st character 'c' to the end, obtaining the string "bac".
In the second move, we move the 1st character 'b' to the end, obtaining the final result "acb".

Example 2:

Input: s = "baaca", k = 3
Output: "aaabc"
Explanation:
In the first move, we move the 1st character 'b' to the end, obtaining the string "aacab".
In the second move, we move the 3rd character 'c' to the end, obtaining the final result "aaabc".

__Constraints:__

1 <= k <= s.length <= 1000
s consist of lowercase English letters.

__题目描述__:
给出了一个由小写字母组成的字符串 S。然后，我们可以进行任意次数的移动。

在每次移动中，我们选择前 K 个字母中的一个（从左侧开始），将其从原位置移除，并放置在字符串的末尾。

返回我们在任意次数的移动之后可以拥有的按字典顺序排列的最小字符串。

__示例 :__

示例 1：

输入：S = "cba", K = 1
输出："acb"
解释：
在第一步中，我们将第一个字符（“c”）移动到最后，获得字符串 “bac”。
在第二步中，我们将第一个字符（“b”）移动到最后，获得最终结果 “acb”。

示例 2：

输入：S = "baaca", K = 3
输出："aaabc"
解释：
在第一步中，我们将第一个字符（“b”）移动到最后，获得字符串 “aacab”。
在第二步中，我们将第三个字符（“c”）移动到最后，获得最终结果 “aaabc”。

__提示:__

1 <= K <= S.length <= 1000
S 只由小写字母组成。

__思路__:

模拟
k == 1 时, 转化为旋转数组找最小值
k > 1 时, 由于每次可以任意选择前 k 个字符放到末尾, 所以只要按照冒泡排序的思路即可选出 s 排序之后的最小值, 直接排序即可
时间复杂度为 O(n ^ 2), 空间复杂度为 O(1), k > 1 时, 可以将时间复杂度减小到 O(nlgn)

__代码__:
__C++__:

```C++
class Solution
{
public:
    string orderlyQueue(string s, int k) 
    {
        if (k == 1)
        {
            string ss = s, result = s;
            for (int i = 0, n = s.size(); i < n - 1; i++) if((ss = ss.substr(1, n - 1) + ss[0]).compare(0, n, result) < 0) result = ss;
            return result;
        }
        sort(s.begin(), s.end());
        return s;
    }
};
```

__Java__:

```Java
class Solution {
    public String orderlyQueue(String s, int k) {
        if (k == 1) {
            String result = s;
            for (int i = 0, n = s.length(); i < n; i++) {
                s = s.substring(1, n) + s.substring(0, 1);
                result = result.compareTo(s) < 0 ? result : s;
            }
            return result;
        }
        char[] chars = s.toCharArray();
        Arrays.sort(chars);
        return new String(chars);
    }
}
```

__Python__:

```Python
class Solution:
    def orderlyQueue(self, s: str, k: int) -> str:
        return "".join(sorted(s)) if k > 1 else min(s[i:] + s[:i] for i in range(len(s)))
```
