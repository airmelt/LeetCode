# 2582 Pass the Pillow 递枕头

__Description:__

There are `n` people standing in a line labeled from `1` to `n`. The first person in the line is holding a pillow initially. Every second, the person holding the pillow passes it to the next person standing in the line. Once the pillow reaches the end of the line, the direction changes, and people continue passing the pillow in the opposite direction.

- For example, once the pillow reaches the `n ^ th` person they pass it to the `n - 1 ^ th` person, then to the `n - 2 ^ th` person and so on.

Given the two positive integers `n` and `time`, return _the index of the person holding the pillow after_ `time` _seconds_.

__Example:__

Example 1:

```text
Input: n = 4, time = 5
Output: 2
Explanation: People pass the pillow in the following way: 1 -> 2 -> 3 -> 4 -> 3 -> 2.
After five seconds, the 2nd person is holding the pillow.
```

Example 2:

```text
Input: n = 3, time = 2
Output: 3
Explanation: People pass the pillow in the following way: 1 -> 2 -> 3.
After two seconds, the 3rd person is holding the pillow.
```

__Constraints:__

- `2 <= n <= 1000`
- `1 <= time <= 1000`

Note: This question is the same as 3178: Find the Child Who Has the Ball After K Seconds.

__题目描述:__

`n` 个人站成一排，按从 `1` 到 `n` 编号。最初，排在队首的第一个人拿着一个枕头。每秒钟，拿着枕头的人会将枕头传递给队伍中的下一个人。一旦枕头到达队首或队尾，传递方向就会改变，队伍会继续沿相反方向传递枕头。

- 例如，当枕头到达第 `n` 个人时，TA 会将枕头传递给第 `n - 1` 个人，然后传递给第 `n - 2` 个人，依此类推。

给你两个正整数 `n` 和 `time` ，返回 `time` 秒后拿着枕头的人的编号。

__示例:__

示例 1：

```text
输入：n = 4, time = 5
输出：2
解释：队伍中枕头的传递情况为：1 -> 2 -> 3 -> 4 -> 3 -> 2 。
5 秒后，枕头传递到第 2 个人手中。
```

示例 2：

```text
输入：n = 3, time = 2
输出：3
解释：队伍中枕头的传递情况为：1 -> 2 -> 3 。
2 秒后，枕头传递到第 3 个人手中。
```

__提示：__

- `2 <= n <= 1000`
- `1 <= time <= 1000`

注意：本题与 3178.找出 K 秒后拿着球的孩子 一致。

__思路:__

```text
数学
到达队伍尽头需要 n - 1 的时间
如果 time / (n - 1) & 1 == 0, 则说明在队伍的前半段
如果 time / (n - 1) & 1 == 1, 则说明在队伍的后半段
如果在前半段, 则返回 1 + (time % (n - 1))
如果在后半段, 则返回 n - (time % (n - 1))
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int passThePillow(int n, int time) 
    {
        return time / (n - 1) & 1 ? n - (time %= (n - 1)) : 1 + (time %= (n - 1));
    }
};
```

__Java__:

```Java
class Solution {
    public int passThePillow(int n, int time) {
        return time / (n - 1) % 2 > 0 ? n - (time %= (n - 1)) : 1 + (time %= (n - 1));
    }
}
```

__Python__:

```Python
class Solution:
    def passThePillow(self, n: int, time: int) -> int:
        return n - k[1] if (k := divmod(time, n - 1))[0] & 1 else k[1] + 1
```
