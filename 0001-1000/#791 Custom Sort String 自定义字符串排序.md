# 791 Custom Sort String 自定义字符串排序

__Description__:
You are given two strings order and s. All the words of order are unique and were sorted in some custom order previously.

Permute the characters of s so that they match the order that order was sorted. More specifically, if a character x occurs before a character y in order, then x should occur before y in the permuted string.

Return any permutation of s that satisfies this property.

__Example:__

Example 1:

Input: order = "cba", s = "abcd"
Output: "cbad"
Explanation:
"a", "b", "c" appear in order, so the order of "a", "b", "c" should be "c", "b", and "a".
Since "d" does not appear in order, it can be at any position in the returned string. "dcba", "cdba", "cbda" are also valid outputs.

Example 2:

Input: order = "cbafg", s = "abcd"
Output: "cbad"

__Constraints:__

1 <= order.length <= 26
1 <= s.length <= 200
order and s consist of lowercase English letters.
All the characters of order are unique.

__题目描述__:
字符串S和 T 只包含小写字符。在S中，所有字符只会出现一次。

S 已经根据某种规则进行了排序。我们要根据S中的字符顺序对T进行排序。更具体地说，如果S中x在y之前出现，那么返回的字符串中x也应出现在y之前。

返回任意一种符合条件的字符串T。

__示例 :__

输入:
S = "cba"
T = "abcd"
输出: "cbad"
解释:
S中出现了字符 "a", "b", "c", 所以 "a", "b", "c" 的顺序应该是 "c", "b", "a".
由于 "d" 没有在S中出现, 它可以放在T的任意位置. "dcba", "cdba", "cbda" 都是合法的输出。

__注意:__

S的最大长度为26，其中没有重复的字符。
T的最大长度为200。
S和T只包含小写字符。

__思路__:

1. 哈希表
用哈希表或者一个数组记录 s 的字符个数
先将 order 中出现的字符按序插入到结果
剩下的字符直接接在结果的最后
2. 双指针
用两个指针分别指向 order 和 s
每次找到 order 的字符, 将 order 的字符交换到 s 的前面
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string customSortString(string order, string s) 
    {
        int index = 0;
        for (const auto& c : order) for (int i = index; i < s.size(); i++) if (s[i] == c) swap(s[i], s[index++]);
        return s;
    }
};
```

__Java__:

```Java
class Solution {
    public String customSortString(String order, String s) {
        int[] count = new int[26];
        for (char c: s.toCharArray()) ++count[c - 'a'];
        StringBuilder result = new StringBuilder();
        for (char c: order.toCharArray()) while(count[c - 'a']-- > 0) result.append(c);
        for (char c = 'a'; c <= 'z'; ++c) while(count[c - 'a']-- > 0) result.append(c);
        return result.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def customSortString(self, order: str, s: str) -> str:
        return ''.join(sorted(list(s), key=lambda x: order.find(x)))
```
