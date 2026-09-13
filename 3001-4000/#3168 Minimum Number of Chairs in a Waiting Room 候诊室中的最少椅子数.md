# 3168 Minimum Number of Chairs in a Waiting Room 候诊室中的最少椅子数

__Description:__

You are given a string `s`. Simulate events at each second `i`:

- If `s[i] == 'E'`, a person enters the waiting room and takes one of the chairs in it.
- If `s[i] == 'L'`, a person leaves the waiting room, freeing up a chair.

Return the __minimum__ number of chairs needed so that a chair is available for every person who enters the waiting room given that it is initially __empty__.

__Example:__

Example 1:

```text
Input: s = "EEEEEEE"

Output: 7

Explanation:

After each second, a person enters the waiting room and no person leaves it. Therefore, a minimum of 7 chairs is needed.
```

Example 2:

```text
Input: s = "ELELEEL"

Output: 2

Explanation:

Let's consider that there are 2 chairs in the waiting room. The table below shows the state of the waiting room at each second.
```

| Second | Event | People in the Waiting Room | Available Chairs |
| - | ----- | - | - |
| 0 | Enter | 1 | 1 |
| 1 | Leave | 0 | 2 |
| 2 | Enter | 1 | 1 |
| 3 | Leave | 0 | 2 |
| 4 | Enter | 1 | 1 |
| 5 | Enter | 2 | 0 |
| 6 | Leave | 1 | 1 |

Example 3:

```text
Input: s = "ELEELEELLL"

Output: 3

Explanation:

Let's consider that there are 3 chairs in the waiting room. The table below shows the state of the waiting room at each second.
```

| Second | Event | People in the Waiting Room | Available Chairs |
| - | ----- | - | - |
| 0 | Enter | 1 | 2 |
| 1 | Leave | 0 | 3 |
| 2 | Enter | 1 | 2 |
| 3 | Enter | 2 | 1 |
| 4 | Leave | 1 | 2 |
| 5 | Enter | 2 | 1 |
| 6 | Enter | 3 | 0 |
| 7 | Leave | 2 | 1 |
| 8 | Leave | 1 | 2 |
| 9 | Leave | 0 | 3 |

__Constraints:__

- `1 <= s.length <= 50`
- `s` consists only of the letters `'E'` and `'L'`.
- `s` represents a valid sequence of entries and exits.

__题目描述:__

给你一个字符串 `s`，模拟每秒钟的事件 `i`：

如果 `s[i] == 'E'`，表示有一位顾客进入候诊室并占用一把椅子。
如果 `s[i] == 'L'`，表示有一位顾客离开候诊室，从而释放一把椅子。
返回保证每位进入候诊室的顾客都能有椅子坐的 __最少__ 椅子数，假设候诊室最初是 __空的__ 。

__示例:__

示例 1：

```text
输入：s = "EEEEEEE"

输出：7

解释：

每秒后都有一个顾客进入候诊室，没有人离开。因此，至少需要 7 把椅子。
```

示例 2：

```text
输入：s = "ELELEEL"

输出：2

解释：

假设候诊室里有 2 把椅子。下表显示了每秒钟等候室的状态。
```

| 秒 | 事件 | 候诊室的人数 | 可用的椅子数 |
| - | ----- | - | - |
| 0 | Enter | 1 | 1 |
| 1 | Leave | 0 | 2 |
| 2 | Enter | 1 | 1 |
| 3 | Leave | 0 | 2 |
| 4 | Enter | 1 | 1 |
| 5 | Enter | 2 | 0 |
| 6 | Leave | 1 | 1 |

示例 3：

```text
输入：s = "ELEELEELLL"

输出：3

解释：

假设候诊室里有 3 把椅子。下表显示了每秒钟等候室的状态。
```

| 秒 | 事件 | 候诊室的人数 | 可用的椅子数 |
| - | ----- | - | - |
| 0 | Enter | 1 | 2 |
| 1 | Leave | 0 | 3 |
| 2 | Enter | 1 | 2 |
| 3 | Enter | 2 | 1 |
| 4 | Leave | 1 | 2 |
| 5 | Enter | 2 | 1 |
| 6 | Enter | 3 | 0 |
| 7 | Leave | 2 | 1 |
| 8 | Leave | 1 | 2 |
| 9 | Leave | 0 | 3 |

__提示：__

- `1 <= s.length <= 50`
- `s` 仅由字母 `'E'` 和 `'L'` 组成。
- `s` 表示一个有效的进出序列。

__思路:__

```text
前缀和
将 'E' 转化为 1 将 'L' 转化为 -1
求出前缀和里最大的即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumChairs(string s) 
    {
        int result = 0, pre = 0;
        for (const auto& c : s)
        {
            pre += c == 'E' ? 1 : -1;
            result = max(result, pre);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumChairs(String s) {
        int result = 0, pre = 0;
        for (var c : s.toCharArray()) {
            pre += c == 'E' ? 1 : -1;
            result = Math.max(result, pre);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumChairs(self, s: str) -> int:
        return max(list(accumulate([1 if c == 'E' else -1 for c in s])))
```
