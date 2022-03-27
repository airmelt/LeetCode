# 1079 Letter Tile Possibilities 活字印刷

__Description__:
You have n  tiles, where each tile has one letter tiles[i] printed on it.

Return the number of possible non-empty sequences of letters you can make using the letters printed on those tiles.

__Example:__

Example 1:

Input: tiles = "AAB"
Output: 8
Explanation: The possible sequences are "A", "B", "AA", "AB", "BA", "AAB", "ABA", "BAA".

Example 2:

Input: tiles = "AAABBC"
Output: 188

Example 3:

Input: tiles = "V"
Output: 1

__Constraints:__

1 <= tiles.length <= 7
tiles consists of uppercase English letters.

__题目描述__:
你有一套活字字模 tiles，其中每个字模上都刻有一个字母 tiles[i]。返回你可以印出的非空字母序列的数目。

注意：本题中，每个活字字模只能使用一次。

__示例 :__

示例 1：

输入："AAB"
输出：8
解释：可能的序列为 "A", "B", "AA", "AB", "BA", "AAB", "ABA", "BAA"。

示例 2：

输入："AAABBC"
输出：188

示例 3：

输入："V"
输出：1

__提示:__

1 <= tiles.length <= 7
tiles 由大写英文字母组成

__思路__:

DFS
本质上就是求字符串的非空子集
先计算每一个字符出现的次数
再对出现的次数进行深度优先搜索去重即可
时间复杂度O(2 ^ n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numTilePossibilities(string tiles) 
    {
        vector<int> count(26);
        for (const auto& c : tiles) ++count[c - 'A'];
        return dfs(count);
    }
private:
    int dfs(vector<int>& count)
    {
        int result = 0;
        for (int i = 0; i < 26; i++)
        {
            if (count[i])
            {
                ++result;
                --count[i];
                result += dfs(count);
                ++count[i];
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numTilePossibilities(String tiles) {
        int[] count = new int[26];
        for (char c : tiles.toCharArray()) ++count[c - 'A'];
        return dfs(count);
    }
    
    private int dfs(int[] count) {
        int sum = 0;
        for (int i = 0; i < 26; i++) {
            if (count[i] > 0) {
                sum++;
                count[i]--;
                sum += dfs(count);
                count[i]++;
            }
        }
        return sum;
    }
}
```

__Python__:

```Python
class Solution:
    def numTilePossibilities(self, tiles: str) -> int:
        return len(set([j for i in range(1, len(tiles) + 1) for j in permutations(tiles, i)]))
```
