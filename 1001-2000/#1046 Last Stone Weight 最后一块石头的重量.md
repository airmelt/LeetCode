# 1046 Last Stone Weight 最后一块石头的重量

__Description__:
We have a collection of rocks, each rock has a positive integer weight.

Each turn, we choose the two heaviest rocks and smash them together.  Suppose the stones have weights x and y with x <= y.  The result of this smash is:

If x == y, both stones are totally destroyed;
If x != y, the stone of weight x is totally destroyed, and the stone of weight y has new weight y-x.
At the end, there is at most 1 stone left.  Return the weight of this stone (or 0 if there are no stones left.)

__Example:__

Example 1:

Input: [2,7,4,1,8,1]
Output: 1
Explanation:
We combine 7 and 8 to get 1 so the array converts to [2,4,1,1,1] then,
we combine 2 and 4 to get 2 so the array converts to [2,1,1,1] then,
we combine 2 and 1 to get 1 so the array converts to [1,1,1] then,
we combine 1 and 1 to get 0 so the array converts to [1] then that's the value of last stone.

__Note:__

1 <= stones.length <= 30
1 <= stones[i] <= 1000

__题目描述__:
有一堆石头，每块石头的重量都是正整数。

每一回合，从中选出两块最重的石头，然后将它们一起粉碎。假设石头的重量分别为 x 和 y，且 x <= y。那么粉碎的可能结果如下：

如果 x == y，那么两块石头都会被完全粉碎；
如果 x != y，那么重量为 x 的石头将会完全粉碎，而重量为 y 的石头新重量为 y-x。
最后，最多只会剩下一块石头。返回此石头的重量。如果没有石头剩下，就返回 0。

__提示：__

1 <= stones.length <= 30
1 <= stones[i] <= 1000

__思路__:

1. 按照题目要求, 一直排序, 将最大的两个石头碰撞
时间复杂度O(n ^ 2lgn), 空间复杂度O(1)
2. 由于每次都是对最大的两块石头进行操作, 可以采用大顶堆
时间复杂度O(nlgn), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int lastStoneWeight(vector<int>& stones) 
    {
        priority_queue<int> pq(stones.begin(), stones.end());
        while (pq.size() > 1)
        {
            int first = pq.top();
            pq.pop();
            int second = pq.top();
            pq.pop();
            if (first - second) pq.push(first - second);
        }
        return pq.empty() ? 0 : pq.top();
    }
};
```

__Java__:

```Java
class Solution {
    public int lastStoneWeight(int[] stones) {
        int n = stones.length;
        if (n == 1) return stones[0];
        if (n == 2) return Math.abs(stones[0] - stones[1]);
        Arrays.sort(stones);
        if (stones[n - 3] == 0) return stones[n - 1] - stones[n - 2];
        stones[n - 1] -= stones[n - 2];
        stones[n - 2] = 0;
        return lastStoneWeight(stones);
    }
}
```

__Python__:

```Python
class Solution:
    def lastStoneWeight(self, stones: List[int]) -> int:
        while len(stones) > 1:
            stones.sort()
            if stones[-1] == stones[-2]:
                stones = stones[:-2]
            else:
                stones[-2] = stones[-1] - stones[-2]
                stones.pop(-1)
        return stones[0] if stones else 0
```
