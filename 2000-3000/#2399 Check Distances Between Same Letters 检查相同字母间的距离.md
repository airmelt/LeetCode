# 2399 Check Distances Between Same Letters 检查相同字母间的距离

__Description:__

You are given a __0-indexed__ string `s` consisting of only lowercase English letters, where each letter in `s` appears __exactly twice__. You are also given a __0-indexed__ integer array `distance` of length `26`.

Each letter in the alphabet is numbered from `0` to `25` (i.e. `'a' -> 0`, `'b' -> 1`, `'c' -> 2`, ... , `'z' -> 25`).

In a __well-spaced__ string, the number of letters between the two occurrences of the `i ^ th` letter is `distance[i]`. If the `i ^ th` letter does not appear in `s`, then `distance[i]` can be __ignored__.

Return `true` if `s` is a __well-spaced__ string, otherwise return `false`.

__Example:__

Example 1:

```text
Input: s = "abaccb", distance = [1,3,0,5,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
Output: true
Explanation:
```

- 'a' appears at indices 0 and 2 so it satisfies distance[0] = 1.
- 'b' appears at indices 1 and 5 so it satisfies distance[1] = 3.
- 'c' appears at indices 3 and 4 so it satisfies distance[2] = 0.

Note that distance[3] = 5, but since 'd' does not appear in s, it can be ignored.

Return true because s is a well-spaced string.

Example 2:

```text
Input: s = "aa", distance = [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
Output: false
Explanation:
```

- 'a' appears at indices 0 and 1 so there are zero letters between them.

Because distance[0] = 1, s is not a well-spaced string.

__Constraints:__

- `2 <= s.length <= 52`
- `s` consists only of lowercase English letters.
- Each letter appears in `s` exactly twice.
- `distance.length == 26`
- `0 <= distance[i] <= 50`

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `s` ，该字符串仅由小写英文字母组成，`s` 中的每个字母都 __恰好__ 出现 __两次__ 。另给你一个下标从 __0__ 开始、长度为 `26` 的的整数数组 `distance` 。

字母表中的每个字母按从 `0` 到 `25` 依次编号（即，`'a' -> 0`, `'b' -> 1`, `'c' -> 2`, ... , `'z' -> 25`）。

在一个 匀整 字符串中，第 `i` 个字母的两次出现之间的字母数量是 `distance[i]` 。如果第 `i` 个字母没有在 `s` 中出现，那么 `distance[i]` 可以 忽略 。

如果 `s` 是一个 __匀整__ 字符串，返回 `true` ；否则，返回 `false` 。

__示例:__

示例 1：

```text
输入：s = "abaccb", distance = [1,3,0,5,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
输出：true
解释：
```

- 'a' 在下标 0 和下标 2 处出现，所以满足 distance[0] = 1 。
- 'b' 在下标 1 和下标 5 处出现，所以满足 distance[1] = 3 。
- 'c' 在下标 3 和下标 4 处出现，所以满足 distance[2] = 0 。

注意 distance[3] = 5 ，但是由于 'd' 没有在 s 中出现，可以忽略。

因为 s 是一个匀整字符串，返回 true 。

示例 2：

```text
输入：s = "aa", distance = [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
输出：false
解释：
```

- 'a' 在下标 0 和 1 处出现，所以两次出现之间的字母数量为 0 。

但是 distance[0] = 1 ，s 不是一个匀整字符串。

__提示：__

- `2 <= s.length <= 52`
- `s` 仅由小写英文字母组成
- `s` 中的每个字母恰好出现两次
- `distance.length == 26`
- `0 <= distance[i] <= 50`

__思路:__

```text
哈希表/数组
记录每个字母出现的位置
如果已经出现过, 计算两次出现的位置之差
如果两次出现的位置之差不等于 distance[i], 返回 false
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumRobots(vector<int>& chargeTimes, vector<int>& runningCosts, long long budget) 
    {
        int result = 0, left = 0, n = chargeTimes.size();
        long long s = 0LL;
        deque<int> q;
        for (int right = 0; right < n; right++)
        {
            while (!q.empty() and chargeTimes[right] >= chargeTimes[q.back()]) q.pop_back();
            q.push_back(right);
            s += runningCosts[right];
            while (!q.empty() and chargeTimes[q.front()] + (right - left + 1) * s > budget)
            {
                if (q.front() == left) q.pop_front();
                s -= runningCosts[left++];
            }
            result = max(result, right - left + 1);
        }    
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumRobots(int[] chargeTimes, int[] runningCosts, long budget) {
        int result = 0, left = 0, n = chargeTimes.length;
        long s = 0L;
        Deque<Integer> q = new ArrayDeque<>();
        for (int right = 0; right < n; right++) {
            while (!q.isEmpty() && chargeTimes[right] >= chargeTimes[q.peekLast()]) q.pollLast();
            q.addLast(right);
            s += runningCosts[right];
            while (!q.isEmpty() && chargeTimes[q.peekFirst()] + (right - left + 1) * s > budget) {
                if (q.peekFirst() == left) q.pollFirst();
                s -= runningCosts[left++];
            }
            result = Math.max(result, right - left + 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumRobots(self, chargeTimes: List[int], runningCosts: List[int], budget: int) -> int:
        left, right, pre = 1, (n := len(chargeTimes)), [0] + list(accumulate(runningCosts))
        def check(mid: int) -> int:
            result, queue = float('inf'), deque()
            for i in range(n):
                if queue and i - queue[0] >= mid:
                    queue.popleft()
                while queue and chargeTimes[queue[-1]] < chargeTimes[i]:
                    queue.pop()
                queue.append(i)
                if i > mid - 2:
                    result = min(result, chargeTimes[queue[0]] + mid * (pre[i + 1] - pre[i - mid + 1]))
            return result
        while left <= right:
            mid = left + right >> 1
            if check(mid) <= budget:
                left = mid + 1
            else:
                right = mid - 1
        return right
```
