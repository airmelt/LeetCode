# 2260 Minimum Consecutive Cards to Pick Up 必须拿起的最小连续卡牌数

__Description:__

You are given an integer array `cards` where `cards[i]` represents the __value__ of the `i ^ th` card. A pair of cards are __matching__ if the cards have the __same__ value.

Return _the __minimum__ number of __consecutive__ cards you have to pick up to have a pair of __matching__ cards among the picked cards._ If it is impossible to have matching cards, return `-1`.

__Example:__

Example 1:

```text
Input: cards = [3,4,2,3,4,7]
Output: 4
Explanation: We can pick up the cards [3,4,2,3] which contain a matching pair of cards with value 3. Note that picking up the cards [4,2,3,4] is also optimal.
```

Example 2:

```text
Input: cards = [1,0,5,3]
Output: -1
Explanation: There is no way to pick up a set of consecutive cards that contain a pair of matching cards.
```

__Constraints:__

- `1 <= cards.length <= 10 ^ 5`
- `0 <= cards[i] <= 10 ^ 6`

__题目描述:__

给你一个整数数组 `cards` ，其中 `cards[i]` 表示第 `i` 张卡牌的 __值__ 。如果两张卡牌的值相同，则认为这一对卡牌 __匹配__ 。

返回你必须拿起的最小连续卡牌数，以使在拿起的卡牌中有一对匹配的卡牌。如果无法得到一对匹配的卡牌，返回 `-1` 。

__示例:__

示例 1：

```text
输入：cards = [3,4,2,3,4,7]
输出：4
解释：拿起卡牌 [3,4,2,3] 将会包含一对值为 3 的匹配卡牌。注意，拿起 [4,2,3,4] 也是最优方案。
```

示例 2：

```text
输入：cards = [1,0,5,3]
输出：-1
解释：无法找出含一对匹配卡牌的一组连续卡牌。
```

__提示：__

- `1 <= cards.length <= 10 ^ 5`
- `0 <= cards[i] <= 10 ^ 6`

__思路:__

```text
哈希表/滑动窗口
用哈希表记录每个 card 出现的位置
贪心的找上一个相同 card 的位置，计算最小的连续卡牌数
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumCardPickup(vector<int>& cards) 
    {
        int inf = 0x3f3f3f3f, result = inf, n = cards.size();
        unordered_map<int, int> idx;
        for (int i = 0; i < n; i++) 
        {
            if (idx.count(cards[i])) result = min(result, i - idx[cards[i]] + 1);
            idx[cards[i]] = i;
        }
        return result < inf ? result : -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumCardPickup(int[] cards) {
        int inf = 0x3f3f3f3f, result = inf, n = cards.length;
        Map<Integer, Integer> idx = new HashMap<>();
        for (int i = 0; i < n; i++) {
            if (idx.containsKey(cards[i])) result = Math.min(result, i - idx.get(cards[i]) + 1);
            idx.put(cards[i], i);
        }
        return result < inf ? result : -1;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumCardPickup(self, cards: List[int]) -> int:
        idx, result = {}, inf
        for i, card in enumerate(cards):
            if card in idx:
                result = min(result, i - idx[card] + 1)
            idx[card] = i
        return result if result < inf else -1
```
