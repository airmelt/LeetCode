# 1128 Number of Equivalent Domino Pairs 等价多米诺骨牌对的数量

__Description__:
Given a list of dominoes, dominoes[i] = [a, b] is equivalent to dominoes[j] = [c, d] if and only if either (a==c and b==d), or (a==d and b==c) - that is, one domino can be rotated to be equal to another domino.

Return the number of pairs (i, j) for which 0 <= i < j < dominoes.length, and dominoes[i] is equivalent to dominoes[j].

__Example:__

Example 1:

Input: dominoes = [[1,2],[2,1],[3,4],[5,6]]
Output: 1

__Constraints:__

1 <= dominoes.length <= 40000
1 <= dominoes[i][j] <= 9

__题目描述__:
给你一个由一些多米诺骨牌组成的列表 dominoes。

如果其中某一张多米诺骨牌可以通过旋转 0 度或 180 度得到另一张多米诺骨牌，我们就认为这两张牌是等价的。

形式上，dominoes[i] = [a, b] 和 dominoes[j] = [c, d] 等价的前提是 a==c 且 b==d，或是 a==d 且 b==c。

在 0 <= i < j < dominoes.length 的前提下，找出满足 dominoes[i] 和 dominoes[j] 等价的骨牌对 (i, j) 的数量。

__示例 :__

输入：dominoes = [[1,2],[2,1],[3,4],[5,6]]
输出：1

__提示：__

1 <= dominoes.length <= 40000
1 <= dominoes[i][j] <= 9

__思路__:

设置一个 map, 由于 dominoes中的元素两个都小于 9, 可以只考虑将其转化为 2位数. 十位数字放较大的元素, 个位数字放较小的元素, 保证旋转之后相等.
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numEquivDominoPairs(vector<vector<int>>& dominoes) 
    {
        int count[100]{0}, result = 0;
        for (auto domino : dominoes) result += count[max(domino[0], domino[1]) * 10 + min(domino[0], domino[1])]++;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numEquivDominoPairs(int[][] dominoes) {
        int count[] = new int[100], result = 0;
        for (int[] domino : dominoes) result += count[Math.max(domino[0], domino[1]) * 10 + Math.min(domino[0], domino[1])]++;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numEquivDominoPairs(self, dominoes: List[List[int]]) -> int:
        return sum(c * (c - 1) // 2 for c in collections.Counter([tuple(sorted(d)) for d in dominoes]).values())
```
