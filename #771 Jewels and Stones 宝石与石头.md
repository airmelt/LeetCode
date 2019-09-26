__Description__:
You're given strings J representing the types of stones that are jewels, and S representing the stones you have.  Each character in S is a type of stone you have.  You want to know how many of the stones you have are also jewels.

The letters in J are guaranteed distinct, and all characters in J and S are letters. Letters are case sensitive, so "a" is considered a different type of stone from "A".

__Example:__
Example 1:

Input: J = "aA", S = "aAAbbbb"
Output: 3

Example 2:

Input: J = "z", S = "ZZ"
Output: 0

__Note:__

S and J will consist of letters and have length at most 50.
The characters in J are distinct.

__题目描述__:
 给定字符串J 代表石头中宝石的类型，和字符串 S代表你拥有的石头。 S 中每个字符代表了一种你拥有的石头的类型，你想知道你拥有的石头中有多少是宝石。

J 中的字母不重复，J 和 S中的所有字符都是字母。字母区分大小写，因此"a"和"A"是不同类型的石头。

__示例 :__
示例 1:

输入: J = "aA", S = "aAAbbbb"
输出: 3

示例 2:

输入: J = "z", S = "ZZ"
输出: 0

__注意：__

S 和 J 最多含有50个字母。
 J 中的字符不重复。

__思路__:
1. 用数组记录下宝石的种类, 遍历 S即可
2. 正则表达式
时间复杂度O(n ^ 2), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    int numJewelsInStones(string J, string S) {
        int count[128] = {0}, result = 0;
        for (char c : J) count[c]++;
        for (char c : S) if (count[c]) result++;
        return result;
    }
};
```

__Java__:
```
class Solution {
    public int numJewelsInStones(String J, String S) {
        return S.replaceAll("[^" + J + "]", "").length();
    }
}
```

__Python__:
```
class Solution:
    def numJewelsInStones(self, J: str, S: str) -> int:
        return len([i for i in S if i in J])
```