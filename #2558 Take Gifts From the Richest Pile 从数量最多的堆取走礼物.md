# 2558 Take Gifts From the Richest Pile 从数量最多的堆取走礼物

__Description:__

You are given an integer array `gifts` denoting the number of gifts in various piles. Every second, you do the following:

- Choose the pile with the maximum number of gifts.
- If there is more than one pile with the maximum number of gifts, choose any.
- Reduce the number of gifts in the pile to the floor of the square root of the original number of gifts in the pile.

Return _the number of gifts remaining after_ `k` _seconds._

__Example:__

Example 1:

```text
Input: gifts = [25,64,9,4,100], k = 4
Output: 29
Explanation: 
The gifts are taken in the following way:
```

- In the first second, the last pile is chosen and 10 gifts are left behind.
- Then the second pile is chosen and 8 gifts are left behind.
- After that the first pile is chosen and 5 gifts are left behind.
- Finally, the last pile is chosen again and 3 gifts are left behind.

The final remaining gifts are [5,8,9,4,3], so the total number of gifts remaining is 29.

Example 2:

```text
Input: gifts = [1,1,1,1], k = 4
Output: 4
Explanation: 
In this case, regardless which pile you choose, you have to leave behind 1 gift in each pile. 
That is, you can't take any pile with you. 
So, the total gifts remaining are 4.
```

__Constraints:__

- `1 <= gifts.length <= 10 ^ 3`
- `1 <= gifts[i] <= 10 ^ 9`
- `1 <= k <= 10 ^ 3`

__题目描述:__

给你一个整数数组 `gifts` ，表示各堆礼物的数量。每一秒，你需要执行以下操作:

- 选择礼物数量最多的那一堆。
- 如果不止一堆都符合礼物数量最多，从中选择任一堆即可。
- 将堆中的礼物数量减少到堆中原来礼物数量的平方根，向下取整。

返回在 `k` 秒后剩下的礼物数量_。_

__示例:__

示例 1：

```text
输入：gifts = [25,64,9,4,100], k = 4
输出：29
解释： 
按下述方式取走礼物：
```

- 在第一秒，选中最后一堆，剩下 10 个礼物。
- 接着第二秒选中第二堆礼物，剩下 8 个礼物。
- 然后选中第一堆礼物，剩下 5 个礼物。
- 最后，再次选中最后一堆礼物，剩下 3 个礼物。

最后剩下的礼物数量分别是 [5,8,9,4,3] ，所以，剩下礼物的总数量是 29 。

示例 2：

```text
输入：gifts = [1,1,1,1], k = 4
输出：4
解释：
在本例中，不管选中哪一堆礼物，都必须剩下 1 个礼物。 
也就是说，你无法获取任一堆中的礼物。 
所以，剩下礼物的总数量是 4 。
```

__提示：__

- `1 <= gifts.length <= 10 ^ 3`
- `1 <= gifts[i] <= 10 ^ 9`
- `1 <= k <= 10 ^ 3`

__思路:__

```text
优先队列
将数组转换为最大堆
每次取出堆顶元素, 计算平方根后重新入堆
经过 k 次操作后, 将堆中所有元素相加即为结果
时间复杂度为 O(KlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long pickGifts(vector<int>& gifts, int k) 
    {
        priority_queue<int> pq(gifts.begin(), gifts.end());
        while (k--) 
        {
            int x = pq.top(); 
            pq.pop();
            pq.push(int(sqrt(x)));
        }
        long long result = 0LL;
        while (pq.size()) 
        {
            result += pq.top(); 
            pq.pop();
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long pickGifts(int[] gifts, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<Integer>((a, b) -> b - a);
        for (int gift : gifts) pq.offer(gift);
        while (k-- > 0) pq.offer((int)Math.sqrt(pq.poll()));
        long result = 0L;
        while (!pq.isEmpty()) result += pq.poll();
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def pickGifts(self, gifts: List[int], k: int) -> int:
        return sum(gifts) if not k else self.pickGifts(sorted(gifts, reverse=True)[1:] + [int(max(gifts) ** .5)], k - 1)
```
