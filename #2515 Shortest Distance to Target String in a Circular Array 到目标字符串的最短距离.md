# 2515 Shortest Distance to Target String in a Circular Array 到目标字符串的最短距离

__Description:__

You are given a __0-indexed__ __circular__ string array `words` and a string `target`. A __circular array__ means that the array's end connects to the array's beginning.

- Formally, the next element of `words[i]` is `words[(i + 1) % n]` and the previous element of `words[i]` is `words[(i - 1 + n) % n]`, where `n` is the length of `words`.

Starting from `startIndex`, you can move to either the next word or the previous word with `1` step at a time.

Return _the __shortest__ distance needed to reach the string_ `target`. If the string `target` does not exist in `words`, return `-1`.

__Example:__

Example 1:

```text
Input: words = ["hello","i","am","leetcode","hello"], target = "hello", startIndex = 1
Output: 1
Explanation: We start from index 1 and can reach "hello" by
```

- moving 3 units to the right to reach index 4.
- moving 2 units to the left to reach index 4.
- moving 4 units to the right to reach index 0.
- moving 1 unit to the left to reach index 0.

The shortest distance to reach "hello" is 1.

Example 2:

```text
Input: words = ["a","b","leetcode"], target = "leetcode", startIndex = 0
Output: 1
Explanation: We start from index 0 and can reach "leetcode" by
```

- moving 2 units to the right to reach index 3.
- moving 1 unit to the left to reach index 3.

The shortest distance to reach "leetcode" is 1.

Example 3:

```text
Input:  words = ["i","eat","leetcode"], target = "ate", startIndex = 0
Output:  -1
Explanation:  Since "ate" does not exist in `words`, we return -1.
```

__Constraints:__

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 100`
- `words[i]` and `target` consist of only lowercase English letters.
- `0 <= startIndex < words.length`

__题目描述:__

给你一个下标从 __0__ 开始的 __环形__ 字符串数组 `words` 和一个字符串 `target` 。__环形数组__ 意味着数组首尾相连。

- 形式上， `words[i]` 的下一个元素是 `words[(i + 1) % n]` ，而 `words[i]` 的前一个元素是 `words[(i - 1 + n) % n]` ，其中 `n` 是 `words` 的长度。

从 `startIndex` 开始，你一次可以用 `1` 步移动到下一个或者前一个单词。

返回到达目标字符串 `target` 所需的最短距离。如果 `words` 中不存在字符串 `target` ，返回 `-1` 。

__示例:__

示例 1：

```text
输入：words = ["hello","i","am","leetcode","hello"], target = "hello", startIndex = 1
输出：1
解释：从下标 1 开始，可以经由以下步骤到达 "hello" ：
```

- 向右移动 3 个单位，到达下标 4 。
- 向左移动 2 个单位，到达下标 4 。
- 向右移动 4 个单位，到达下标 0 。
- 向左移动 1 个单位，到达下标 0 。

到达 "hello" 的最短距离是 1 。

示例 2：

```text
输入：words = ["a","b","leetcode"], target = "leetcode", startIndex = 0
输出：1
解释：从下标 0 开始，可以经由以下步骤到达 "leetcode" ：
```

- 向右移动 2 个单位，到达下标 3 。
- 向左移动 1 个单位，到达下标 3 。

到达 "leetcode" 的最短距离是 1 。

示例 3：

```text
输入：words = ["i","eat","leetcode"], target = "ate", startIndex = 0
输出：-1
解释：因为 words 中不存在字符串 "ate" ，所以返回 -1 。
```

__提示：__

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 100`
- `words[i]` 和 `target` 仅由小写英文字母组成
- `0 <= startIndex < words.length`

__思路:__

```text
模拟
初始化 result 为 n
遍历 words 数组
如果当前单词等于 target
更新 result 为 min(result, min(abs(i - startIndex), n - abs(i - startIndex)))
如果 result 等于 n, 返回 -1, 否则返回 result
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int closestTarget(vector<string>& words, string target, int startIndex) 
    {
        int n = words.size(), result = n;
        for (int i = 0; i < n; i++) if (words[i] == target) result = min(result, min(abs(i - startIndex), n - abs(i - startIndex)));
        return result == n ? -1 : result;
    }
};
```

__Java__:

```Java
class Solution {
    public int closestTarget(String[] words, String target, int startIndex) {
        int n = words.length, result = n;
        for (int i = 0; i < n; i++) if (words[i].equals(target)) result = Math.min(result, Math.min(Math.abs(i - startIndex), n - Math.abs(i - startIndex)));
        return result == n ? -1 : result;
    }
}
```

__Python__:

```Python
class Solution:
    def closestTarget(self, words: List[str], target: str, startIndex: int) -> int:
        return -1 if target not in words else min((min(abs(i - startIndex), len(words) - abs(i - startIndex)) for i, w in enumerate(words) if w == target))
```
