# 358 Rearrange String k Distance Apart K 距离间隔重排字符串

__Description:__

Given a string `s` and an integer `k`, rearrange `s` such that the same characters are __at least__ distance `k` from each other. If it is not possible to rearrange the string, return an empty string `""`.

__Example:__

Example 1:

```text
Input: s = "aabbcc", k = 3
Output: "abcabc"
Explanation: The same letters are at least a distance of 3 from each other.
```

Example 2:

```text
Input: s = "aaabc", k = 3
Output: ""
Explanation: It is not possible to rearrange the string.
```

Example 3:

```text
Input: s = "aaadbbcc", k = 2
Output: "abacabcd"
Explanation: The same letters are at least a distance of 2 from each other.
```

__Constraints:__

- `1 <= s.length <= 3 * 10 ^ 5`
- `s` consists of only lowercase English letters.
- `0 <= k <= s.length`

__题目描述:__

给你一个非空的字符串 `s` 和一个整数 `k` ，你要将这个字符串 `s` 中的字母进行重新排列，使得重排后的字符串中相同字母的位置间隔距离 __至少__ 为 `k` 。如果无法做到，请返回一个空字符串 `""`。

__示例:__

示例 1：

```text
输入: s = "aabbcc", k = 3
输出: "abcabc" 
解释: 相同的字母在新的字符串中间隔至少 3 个单位距离。
```

示例 2：

```text
输入: s = "aaabc", k = 3
输出: "" 
解释: 没有办法找到可能的重排结果。
```

示例 3：

```text
输入: s = "aaadbbcc", k = 2
输出: "abacabcd"
解释: 相同的字母在新的字符串中间隔至少 2 个单位距离。
```

__提示：__

- `1 <= s.length <= 3 * 10 ^ 5`
- `s` 仅由小写英文字母组成
- `0 <= k <= s.length`

__思路:__

```text
大顶堆 ➕ 队列
统计每个字符出现的次数, 使用大顶堆进行排序
每次取出堆顶频率元素, 并将其次数减一, 将其加入队列
将队列中的字符元素加入结果字符串中
如果队列长度等于 k, 则取出队首元素, 如果其次数大于 0, 则将其加入堆中
时间复杂度为 O(MlogN + NlogM), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string rearrangeString(string s, int k)
    {
        if (!k) return s;
        priority_queue<pair<int, char>> pq;
        unordered_map<char, int> m;
        queue<pair<int, char>> q;
        string result;
        for (const auto& c : s) ++m[c];
        for (const auto& [k, v] : m) pq.emplace(v, k);
        while (!pq.empty())
        {
            auto [i, c] = pq.top();
            pq.pop();
            result += c;
            q.emplace(i - 1, c);
            if (q.size() == k)
            {
                if (q.front().first > 0) pq.emplace(q.front());
                q.pop();
            }
        }
        return result.size() == s.size() ? result : "";
    }
};
```

__Java__:

```Java
class Solution {
    public String rearrangeString(String s, int k) {
        if (k == 0) return s;
        PriorityQueue<Map.Entry<Character, Integer>> pq = new PriorityQueue<>((a, b) -> b.getValue() - a.getValue());
        Map<Character, Integer> map = new HashMap<>();
        Queue<Map.Entry<Character, Integer>> queue = new LinkedList<>();
        StringBuilder result = new StringBuilder(s.length());
        for (char c : s.toCharArray()) map.merge(c, 1, Integer::sum);
        pq.addAll(map.entrySet());
        while (!pq.isEmpty()) {
            var cur = pq.poll();
            result.append(cur.getKey());
            cur.setValue(cur.getValue() - 1);
            queue.offer(cur);
            if (queue.size() == k) {
                var entry = queue.poll();
                if (entry.getValue() > 0) pq.offer(entry);
            }
        }
        return result.length() == s.length() ? result.toString() : "";
    }
}
```

__Python__:

```Python
class Solution:
    def rearrangeString(self, s: str, k: int) -> str:
        queue, heap, result = deque(), [(-1 * v, k) for k, v in Counter(s).items()], ""
        heapify(heap)
        while heap:
            f, c = heappop(heap)
            result += c
            queue.append((-f - 1, c))
            if len(queue) == k:
                f, c = queue.popleft()
                if f > 0:
                    heappush(heap, (-f, c))
        return s if not k else result if len(result) == len(s) else ""
```
