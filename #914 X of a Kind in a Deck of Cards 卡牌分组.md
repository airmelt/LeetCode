# 914 X of a Kind in a Deck of Cards 卡牌分组

__Description__:
In a deck of cards, each card has an integer written on it.

Return true if and only if you can choose X >= 2 such that it is possible to split the entire deck into 1 or more groups of cards, where:

Each group has exactly X cards.
All the cards in each group have the same integer.

__Example:__

Example 1:

Input: [1,2,3,4,4,3,2,1]
Output: true
Explanation: Possible partition [1,1],[2,2],[3,3],[4,4]

Example 2:

Input: [1,1,1,2,2,2,3,3]
Output: false
Explanation: No possible partition.

Example 3:

Input: [1]
Output: false
Explanation: No possible partition.

Example 4:

Input: [1,1]
Output: true
Explanation: Possible partition [1,1]

Example 5:

Input: [1,1,2,2,2,2]
Output: true
Explanation: Possible partition [1,1],[2,2],[2,2]

__Note:__

1 <= deck.length <= 10000
0 <= deck[i] < 10000

__题目描述__:
给定一副牌，每张牌上都写着一个整数。

此时，你需要选定一个数字 X，使我们可以将整副牌按下述规则分成 1 组或更多组：

每组都有 X 张牌。
组内所有的牌上都写着相同的整数。
仅当你可选的 X >= 2 时返回 true。

__示例 :__

示例 1：

输入：[1,2,3,4,4,3,2,1]
输出：true
解释：可行的分组是 [1,1]，[2,2]，[3,3]，[4,4]

示例 2：

输入：[1,1,1,2,2,2,3,3]
输出：false
解释：没有满足要求的分组。

示例 3：

输入：[1]
输出：false
解释：没有满足要求的分组。

示例 4：

输入：[1,1]
输出：true
解释：可行的分组是 [1,1]

示例 5：

输入：[1,1,2,2,2,2]
输出：true
解释：可行的分组是 [1,1]，[2,2]，[2,2]

__提示：__

1 <= deck.length <= 10000
0 <= deck[i] < 10000

__思路__:

用 map(Counter)记录下每个元素的出现次数, 取所有出现次数的最小公倍数, 返回最小公倍数是否大于等于 2即可
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool hasGroupsSizeX(vector<int>& deck) 
    {
        map<int, int> m;
        for (auto num : deck) m[num]++;
        int result = m[deck[0]];
        for (auto item : m) 
        {
            result = gcd(result, item.second);
            if (result == 1) return false;
        }
        return true;
    }
private:
    int gcd(int a, int b) 
    {
        return a > b ? gcd(b, a) : (a == 0 ? b : gcd(b % a, a));
    }
};
```

__Java__:

```Java
class Solution {
    public boolean hasGroupsSizeX(int[] deck) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : deck) map.put(num, map.getOrDefault(num, 0) + 1);
        int result = map.get(deck[0]);
        for (int value : map.values()) result = gcd(result, value);
        return result > 1;
    }
    
    private int gcd(int a, int b) {
        return a > b ? gcd(b, a) : (a == 0 ? b : gcd(b % a, a));
    }
}
```

__Python__:

```Python
class Solution:
    def hasGroupsSizeX(self, deck: List[int]) -> bool:
        c = collections.Counter(deck)
        return any((all((d % i == 0 for d in c.values())) for i in range(2, 5000)))
```
