# 846 Hand of Straights 一手顺子

__Description__:
Alice has some number of cards and she wants to rearrange the cards into groups so that each group is of size groupSize, and consists of groupSize consecutive cards.

Given an integer array hand where hand[i] is the value written on the ith card and an integer groupSize, return true if she can rearrange the cards, or false otherwise.

__Example:__

Example 1:

Input: hand = [1,2,3,6,2,3,4,7,8], groupSize = 3
Output: true
Explanation: Alice's hand can be rearranged as [1,2,3],[2,3,4],[6,7,8]

Example 2:

Input: hand = [1,2,3,4,5], groupSize = 4
Output: false
Explanation: Alice's hand can not be rearranged into groups of 4.

__Constraints:__

1 <= hand.length <= 10^4
0 <= hand[i] <= 10^9
1 <= groupSize <= hand.length

__Note:__
This question is the same as [1296](https://leetcode.com/problems/divide-array-in-sets-of-k-consecutive-numbers/)

__题目描述__:
爱丽丝有一手（`hand`）由整数数组给定的牌。

现在她想把牌重新排列成组，使得每个组的大小都是 `W`，且由 `W` 张连续的牌组成。

如果她可以完成分组就返回 `true`，否则返回 `false`。

**注意：**此题目与 1296 重复：[https://leetcode-cn.com/problems/divide-array-in-sets-of-k-consecutive-numbers/](https://leetcode-cn.com/problems/divide-array-in-sets-of-k-consecutive-numbers/)

**示例 1：**

**输入：**hand = [1,2,3,6,2,3,4,7,8], W = 3
**输出：**true
**解释：**爱丽丝的手牌可以被重新排列为 `[1,2,3]，[2,3,4]，[6,7,8]`。

**示例 2：**

**输入：**hand = [1,2,3,4,5], W = 4
**输出：**false
**解释：**爱丽丝的手牌无法被重新排列成几个大小为 4 的组。

**提示：**

* `1 <= hand.length <= 10000`
* `0 <= hand[i] <= 10^9`
* `1 <= W <= hand.length`

__思路__:

有序哈希表
使用 multiset(C++)/TreeMap(Java)/sortedcontainers.SortedDict(Python) 记录每个元素出现次数
固定左侧山脚 left, left + 2 < n, 山脉长度至少为 3, 且 arr[left] < arr[left + 1], 否则查找下一个元素
右侧山脚从 left + 1 开始查找, 找到第一个不再递增的元素, 这时为山顶, 判断是否为山顶再找到第一个不递减的元素, 就为右侧山脚, 更新结果长度
将 right 赋给 left 继续查找下一个山脉
时间复杂度为 O(nlgn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isNStraightHand(vector<int>& hand, int groupSize) 
    {
        multiset<int> s;
        for (const auto& c : hand) s.insert(c);
        while (!s.empty())
        {
            int x = *s.begin();
            for (int i = x; i < groupSize + x; i++)
            {
                if (!s.count(i)) return false;
                else s.erase(s.find(i));
            }
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isNStraightHand(int[] hand, int groupSize) {
        TreeMap<Integer, Integer> count = new TreeMap();
        for (int card : hand) count.put(card, count.getOrDefault(card, 0) + 1);
        while (count.size() > 0) {
            int first = count.firstKey();
            for (int card = first; card < first + groupSize; ++card) {
                if (!count.containsKey(card)) return false;
                int c = count.get(card);
                if (c == 1) count.remove(card);
                else count.replace(card, c - 1);
            }
        }
        return true;
    }
}
```

__Python__:

```Python
from sortedcontainers import SortedDict
class Solution:
    def isNStraightHand(self, hand: List[int], groupSize: int) -> bool:
        if len(hand) % groupSize:
            return False
        treemap = SortedDict()
        for num in hand:
            treemap[num] = treemap.get(num, 0) + 1
        while treemap:
            num = treemap.peekitem(0)[0]
            for num in range(num, num + groupSize):
                if num in treemap:
                    treemap[num] -= 1
                    if treemap[num] == 0:
                        del treemap[num]
                else:
                    return False
        return True
```
