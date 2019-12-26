__Description__:
Given a string text, you want to use the characters of text to form as many instances of the word "balloon" as possible.

You can use each character in text at most once. Return the maximum number of instances that can be formed.

__Example:__
Example 1:
![string 1](https://assets.leetcode.com/uploads/2019/09/05/1536_ex1_upd.JPG)
Input: text = "nlaebolko"
Output: 1

Example 2:
![string 2](https://assets.leetcode.com/uploads/2019/09/05/1536_ex2_upd.JPG)
Input: text = "loonbalxballpoon"
Output: 2

Example 3:

Input: text = "leetcode"
Output: 0
 
__Constraints:__

1 <= text.length <= 10^4
text consists of lower case English letters only.

__题目描述__:
给你一个字符串 text，你需要使用 text 中的字母来拼凑尽可能多的单词 "balloon"（气球）。

字符串 text 中的每个字母最多只能被使用一次。请你返回最多可以拼凑出多少个单词 "balloon"。

__示例 :__
示例 1：
![字符串1](https://assets.leetcode.com/uploads/2019/09/05/1536_ex1_upd.JPG)
输入：text = "nlaebolko"
输出：1

示例 2：
![字符串2](https://assets.leetcode.com/uploads/2019/09/05/1536_ex2_upd.JPG)
输入：text = "loonbalxballpoon"
输出：2

示例 3：

输入：text = "leetcode"
输出：0
 
__提示：__

1 <= text.length <= 10^4
text 全部由小写英文字母组成

__思路__:
balloon由 1个 'b', 1个 'a', 2个 'l', 2个 'o', 1个 'n'组成, 只要找到这些字符的个数的最小值即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution {
public:
    int maxNumberOfBalloons(string text) {
        int count[26]{0}, index[]{0, 1, 11, 13, 14}, result = 10000;
        for (auto c : text) ++count[c - 'a'];
        count[11] /= 2;
        count[14] /= 2;
        for (auto i : index) result = min(result, count[i]);
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int maxNumberOfBalloons(String text) {
        int count[] = new int[26], index[] = new int[]{0, 1, 11, 13, 14}, result = Integer.MAX_VALUE;
        for (char c : text.toCharArray()) ++count[c - 'a'];
        count[11] /= 2;
        count[14] /= 2;
        for (int i : index) result = Math.min(result, count[i]);
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def maxNumberOfBalloons(self, text: str) -> int:
        return min(collections.Counter(text)['b'], collections.Counter(text)['a'], collections.Counter(text)['l'] // 2, collections.Counter(text)['o'] // 2, collections.Counter(text)['n'])
```