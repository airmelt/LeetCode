# 1996 The Number of Weak Characters in the Game 游戏中弱角色的数量

__Description:__

You are playing a game that contains multiple characters, and each of the characters has __two__ main properties: __attack__ and __defense__. You are given a 2D integer array `properties` where `properties[i] = [attacki, defensei]` represents the properties of the `i ^ th` character in the game.

A character is said to be __weak__ if any other character has __both__ attack and defense levels __strictly greater__ than this character's attack and defense levels. More formally, a character `i` is said to be __weak__ if there exists another character `j` where `attackj > attacki` and `defensej > defensei`.

Return the number of weak characters.

__Example:__

Example 1:

```text
Input: properties = [[5,5],[6,3],[3,6]]
Output: 0
Explanation: No character has strictly greater attack and defense than the other.
```

Example 2:

```text
Input: properties = [[2,2],[3,3]]
Output: 1
Explanation: The first character is weak because the second character has a strictly greater attack and defense.
```

Example 3:

```text
Input: properties = [[1,5],[10,4],[4,3]]
Output: 1
Explanation: The third character is weak because the second character has a strictly greater attack and defense.
```

__Constraints:__

- `2 <= properties.length <= 10 ^ 5`
- `properties[i].length == 2`
- `1 <= attacki, defensei <= 10 ^ 5`

__题目描述:__

你正在参加一个多角色游戏，每个角色都有两个主要属性:__攻击__ 和 __防御__ 。给你一个二维整数数组 `properties` ，其中 `properties[i] = [attacki, defensei]` 表示游戏中第 `i` 个角色的属性。

如果存在一个其他角色的攻击和防御等级 __都严格高于__ 该角色的攻击和防御等级，则认为该角色为 __弱角色__ 。更正式地，如果认为角色 `i` __弱于__ 存在的另一个角色 `j` ，那么 `attackj > attacki` 且 `defensej > defensei` 。

返回 弱角色 的数量。

__示例:__

示例 1：

```text
输入：properties = [[5,5],[6,3],[3,6]]
输出：0
解释：不存在攻击和防御都严格高于其他角色的角色。
```

示例 2：

```text
输入：properties = [[2,2],[3,3]]
输出：1
解释：第一个角色是弱角色，因为第二个角色的攻击和防御严格大于该角色。
```

示例 3：

```text
输入：properties = [[1,5],[10,4],[4,3]]
输出：1
解释：第三个角色是弱角色，因为第二个角色的攻击和防御严格大于该角色。
```

__提示：__

- `2 <= properties.length <= 10 ^ 5`
- `properties[i].length == 2`
- `1 <= attacki, defensei <= 10 ^ 5`

__思路:__

```text
排序
按照攻击力降序排序, 如果攻击力相同, 则按照防御力升序排序
那么对于每一个角色, 只需要比较其防御力是否小于之前的最大防御力即可
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfWeakCharacters(vector<vector<int>>& properties) 
    {
        sort(properties.begin(), properties.end(), [](const auto& p1, const auto& p2) { return p1.front() == p2.front() ? p1.back() < p2.back() : p1.front() > p2.front(); });
        int cur = 0, result = 0;
        for (const auto& p : properties)
        {
            if (p.back() < cur) ++result;
            else cur = p.back();
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfWeakCharacters(int[][] properties) {
        Arrays.sort(properties, (p1, p2) -> { return p1[0] == p2[0] ? (p1[1] - p2[1]) : (p2[0] - p1[0]); });
        int cur = 0, result = 0;
        for (int[] p : properties) {
            if (p[1] < cur) ++result;
            else cur = p[1];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfWeakCharacters(self, properties: List[List[int]]) -> int:
        properties.sort(key=lambda p: (-p[0], p[1]))
        result = cur = 0
        for atk, d in properties:
            if d < cur:
                result += 1
            else:
                cur = d
        return result
```
