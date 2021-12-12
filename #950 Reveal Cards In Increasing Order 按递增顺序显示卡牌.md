# 950 Reveal Cards In Increasing Order 按递增顺序显示卡牌

__Description__:
You are given an integer array deck. There is a deck of cards where every card has a unique integer. The integer on the ith card is deck[i].

You can order the deck in any order you want. Initially, all the cards start face down (unrevealed) in one deck.

You will do the following steps repeatedly until all cards are revealed:

Take the top card of the deck, reveal it, and take it out of the deck.
If there are still cards in the deck then put the next top card of the deck at the bottom of the deck.
If there are still unrevealed cards, go back to step 1. Otherwise, stop.
Return an ordering of the deck that would reveal the cards in increasing order.

Note that the first entry in the answer is considered to be the top of the deck.

__Example:__

Example 1:

Input: deck = [17,13,11,2,3,5,7]
Output: [2,13,3,11,5,17,7]
Explanation:
We get the deck in the order [17,13,11,2,3,5,7] (this order does not matter), and reorder it.
After reordering, the deck starts as [2,13,3,11,5,17,7], where 2 is the top of the deck.
We reveal 2, and move 13 to the bottom.  The deck is now [3,11,5,17,7,13].
We reveal 3, and move 11 to the bottom.  The deck is now [5,17,7,13,11].
We reveal 5, and move 17 to the bottom.  The deck is now [7,13,11,17].
We reveal 7, and move 13 to the bottom.  The deck is now [11,17,13].
We reveal 11, and move 17 to the bottom.  The deck is now [13,17].
We reveal 13, and move 17 to the bottom.  The deck is now [17].
We reveal 17.
Since all the cards revealed are in increasing order, the answer is correct.

Example 2:

Input: deck = [1,1000]
Output: [1,1000]

__Constraints:__

1 <= deck.length <= 1000
1 <= deck[i] <= 10^6
All the values of deck are unique.

__题目描述__:
牌组中的每张卡牌都对应有一个唯一的整数。你可以按你想要的顺序对这套卡片进行排序。

最初，这些卡牌在牌组里是正面朝下的（即，未显示状态）。

现在，重复执行以下步骤，直到显示所有卡牌为止：

从牌组顶部抽一张牌，显示它，然后将其从牌组中移出。
如果牌组中仍有牌，则将下一张处于牌组顶部的牌放在牌组的底部。
如果仍有未显示的牌，那么返回步骤 1。否则，停止行动。
返回能以递增顺序显示卡牌的牌组顺序。

答案中的第一张牌被认为处于牌堆顶部。

__示例 :__

输入：[17,13,11,2,3,5,7]
输出：[2,13,3,11,5,17,7]
解释：
我们得到的牌组顺序为 [17,13,11,2,3,5,7]（这个顺序不重要），然后将其重新排序。
重新排序后，牌组以 [2,13,3,11,5,17,7] 开始，其中 2 位于牌组的顶部。
我们显示 2，然后将 13 移到底部。牌组现在是 [3,11,5,17,7,13]。
我们显示 3，并将 11 移到底部。牌组现在是 [5,17,7,13,11]。
我们显示 5，然后将 17 移到底部。牌组现在是 [7,13,11,17]。
我们显示 7，并将 13 移到底部。牌组现在是 [11,17,13]。
我们显示 11，然后将 17 移到底部。牌组现在是 [13,17]。
我们展示 13，然后将 17 移到底部。牌组现在是 [17]。
我们显示 17。
由于所有卡片都是按递增顺序排列显示的，所以答案是正确的。

__提示:__

1 <= A.length <= 1000
1 <= A[i] <= 10^6
对于所有的 i != j，A[i] != A[j]

__思路__:

模拟
先排序
然后按照题意要求反向将卡牌插入队列
时间复杂度为 O(nlgn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> deckRevealedIncreasing(vector<int>& deck) 
    {
        sort(deck.begin(), deck.end(), greater<int>());
        queue<int> q;
        for (int i = 0; i < deck.size(); i++) 
        {
            if (!q.empty()) 
            {
                q.push(q.front());
                q.pop();
            }
            q.push(deck[i]);
        }
        for (int i = deck.size() - 1; i > -1; i--)
        {
            deck[i] = q.front();
            q.pop();
        }
        return deck;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] deckRevealedIncreasing(int[] deck) {
        Arrays.sort(deck);
        Queue<Integer> queue = new LinkedList<>();
        for (int i = deck.length - 1; i > -1; i--) {
            if (!queue.isEmpty()) {
                queue.add(queue.poll());
            }
            queue.add(deck[i]);
        }
        for (int i = deck.length - 1; i > -1; i--) deck[i] = queue.poll();
        return deck;
    }
}
```

__Python__:

```Python
class Solution:
    def deckRevealedIncreasing(self, deck: List[int]) -> List[int]:
        queue, n = deque(), len(deck)
        deck.sort(reverse=True)
        for i in range(n):
            (queue and queue.append(queue.popleft())) or queue.append(deck[i])
        return list(queue)[::-1]
```
