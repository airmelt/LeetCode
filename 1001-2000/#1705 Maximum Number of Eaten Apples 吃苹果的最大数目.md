# 1705 Maximum Number of Eaten Apples 吃苹果的最大数目

__Description:__

There is a special kind of apple tree that grows apples every day for `n` days. On the `i ^ th` day, the tree grows `apples[i]` apples that will rot after `days[i]` days, that is on day `i + days[i]` the apples will be rotten and cannot be eaten. On some days, the apple tree does not grow any apples, which are denoted by `apples[i] == 0` and `days[i] == 0`.

You decided to eat __at most__ one apple a day (to keep the doctors away). Note that you can keep eating after the first `n` days.

Given two integer arrays `days` and `apples` of length `n`, return _the maximum number of apples you can eat._

__Example:__

Example 1:

```text
Input: apples = [1,2,3,5,2], days = [3,2,1,4,2]
Output: 7
Explanation: You can eat 7 apples:
- On the first day, you eat an apple that grew on the first day.
- On the second day, you eat an apple that grew on the second day.
- On the third day, you eat an apple that grew on the second day. After this day, the apples that grew on the third day rot.
- On the fourth to the seventh days, you eat apples that grew on the fourth day.
```

Example 2:

```text
Input: apples = [3,0,0,0,0,2], days = [3,0,0,0,0,2]
Output: 5
Explanation: You can eat 5 apples:
- On the first to the third day you eat apples that grew on the first day.
- Do nothing on the fouth and fifth days.
- On the sixth and seventh days you eat apples that grew on the sixth day.
```

__Constraints:__

- `n == apples.length == days.length`
- `1 <= n <= 2 * 10 ^ 4`
- `0 <= apples[i], days[i] <= 2 * 10 ^ 4`
- `days[i] = 0` if and only if `apples[i] = 0`.

__题目描述:__

有一棵特殊的苹果树，一连 `n` 天，每天都可以长出若干个苹果。在第 `i` 天，树上会长出 `apples[i]` 个苹果，这些苹果将会在 `days[i]` 天后（也就是说，第 `i + days[i]` 天时）腐烂，变得无法食用。也可能有那么几天，树上不会长出新的苹果，此时用 `apples[i] == 0` 且 `days[i] == 0` 表示。

你打算每天 __最多__ 吃一个苹果来保证营养均衡。注意，你可以在这 `n` 天之后继续吃苹果。

给你两个长度为 `n` 的整数数组 `days` 和 `apples` ，返回你可以吃掉的苹果的最大数目。

__示例:__

示例 1：

```text
输入：apples = [1,2,3,5,2], days = [3,2,1,4,2]
输出：7
解释：你可以吃掉 7 个苹果：
- 第一天，你吃掉第一天长出来的苹果。
- 第二天，你吃掉一个第二天长出来的苹果。
- 第三天，你吃掉一个第二天长出来的苹果。过了这一天，第三天长出来的苹果就已经腐烂了。
- 第四天到第七天，你吃的都是第四天长出来的苹果。
```

示例 2：

```text
输入：apples = [3,0,0,0,0,2], days = [3,0,0,0,0,2]
输出：5
解释：你可以吃掉 5 个苹果：
- 第一天到第三天，你吃的都是第一天长出来的苹果。
- 第四天和第五天不吃苹果。
- 第六天和第七天，你吃的都是第六天长出来的苹果。
```

__提示：__

- `apples.length == n`
- `days.length == n`
- `1 <= n <= 2 * 10 ^ 4`
- `0 <= apples[i], days[i] <= 2 * 10 ^ 4`
- 只有在 `apples[i] = 0` 时， `days[i] = 0` 才成立

__思路:__

```text
优先队列
将过期日期加入优先队列
先消耗快要过期的苹果
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution {
public:
    int eatenApples(vector<int>& apples, vector<int>& days) 
    {
        priority_queue<pair<int, int>> pq;
        int result = 0;
        for (int i = 0, n = apples.size(); i < n or !pq.empty(); i++) 
        {
            if (i < n and apples[i]) pq.emplace(-(i + days[i]), apples[i]);
            while (!pq.empty() and -pq.top().first <= i) pq.pop();
            if (!pq.empty()) 
            {
                ++result;
                auto [x, y] = pq.top();
                pq.pop();
                if (y > 1) pq.emplace(x, y - 1);
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int eatenApples(int[] apples, int[] days) {
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        int result = 0;
        for (int i = 0, n = apples.length; i < n || !pq.isEmpty(); i++) {
            if (i < n && apples[i] != 0) pq.offer(new int[]{i + days[i], apples[i]});
            while (!pq.isEmpty() && pq.peek()[0] <= i) pq.poll();
            if (!pq.isEmpty()) {
                ++result;
                int[] cur = pq.poll();
                if (--cur[1] > 0) pq.offer(cur);
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def eatenApples(self, apples: List[int], days: List[int]) -> int:
        n, heap, cur, result = len(apples), [], 0, 0
        for i in range(n):
            heappush(heap, [i + days[i], apples[i]])
            while heap and (heap[0][0] <= cur or not heap[0][1]):
                heappop(heap)
            if heap:
                result += 1
                heap[0][1] -= 1
            cur += 1
        while heap:
            while heap and (heap[0][0] <= cur or not heap[0][1]):
                heappop(heap)
            if heap:
                result += min(heap[0][1], heap[0][0] - cur)
                cur += min(heap[0][1], heap[0][0] - cur)
                heappop(heap)
        return result
```
