# 2224 Minimum Number of Operations to Convert Time 转化时间需要的最少操作数

__Description:__

You are given two strings `current` and `correct` representing two __24-hour times__.

24-hour times are formatted as `"HH:MM"`, where `HH` is between `00` and `23`, and `MM` is between `00` and `59`. The earliest 24-hour time is `00:00`, and the latest is `23:59`.

In one operation you can increase the time `current` by `1`, `5`, `15`, or `60` minutes. You can perform this operation __any__ number of times.

Return _the __minimum number of operations__ needed to convert_ `current` _to_ `correct`.

__Example:__

Example 1:

```text
Input: current = "02:30", correct = "04:35"
Output: 3
Explanation:
We can convert current to correct in 3 operations as follows:
```

- Add 60 minutes to current. current becomes "03:30".
- Add 60 minutes to current. current becomes "04:30".
- Add 5 minutes to current. current becomes "04:35".

It can be proven that it is not possible to convert current to correct in fewer than 3 operations.

Example 2:

```text
Input: current = "11:00", correct = "11:01"
Output: 1
Explanation: We only have to add one minute to current, so the minimum number of operations needed is 1.
```

__Constraints:__

- `current` and `correct` are in the format `"HH:MM"`
- `current <= correct`

__题目描述:__

给你两个字符串 `current` 和 `correct` ，表示两个 __24 小时制时间__ 。

__24 小时制时间__ 按 `"HH:MM"` 进行格式化，其中 `HH` 在 `00` 和 `23` 之间，而 `MM` 在 `00` 和 `59` 之间。最早的 24 小时制时间为 `00:00` ，最晚的是 `23:59` 。

在一步操作中，你可以将 `current` 这个时间增加 `1`、 `5`、 `15` 或 `60` 分钟。你可以执行这一操作 __任意__ 次数。

返回将 `current` 转化为 `correct` 需要的 __最少操作数__ 。

__示例:__

示例 1：

```text
输入：current = "02:30", correct = "04:35"
输出：3
解释：
可以按下述 3 步操作将 current 转换为 correct ：
```

- 为 current 加 60 分钟，current 变为 "03:30" 。
- 为 current 加 60 分钟，current 变为 "04:30" 。
- 为 current 加 5 分钟，current 变为 "04:35" 。

可以证明，无法用少于 3 步操作将 current 转化为 correct 。

示例 2：

```text
输入：current = "11:00", correct = "11:01"
输出：1
解释：只需要为 current 加一分钟，所以最小操作数是 1 。
```

__提示：__

- `current` 和 `correct` 都符合 `"HH:MM"` 格式
- `current <= correct`

__思路:__

```text
模拟
计算两个时间的分钟数的差值
然后根据差值计算需要的操作数
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution
{
public:
    int convertTime(string current, string correct) 
    {
        int hours = (correct[0] - '0') * 10 + (correct[1] - '0') - (current[0] - '0') * 10 - (current[1] - '0'), minutes = hours * 60 + (correct[3] - '0') * 10 + (correct[4] - '0') - (current[3] - '0') * 10 - (current[4] - '0'), result = minutes / 60 + minutes % 60 / 15 + minutes % 15 / 5 + minutes % 5;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int convertTime(String current, String correct) {
        int hours = (correct.charAt(0) - '0') * 10 + (correct.charAt(1) - '0') - (current.charAt(0) - '0') * 10 - (current.charAt(1) - '0'), minutes = hours * 60 + (correct.charAt(3) - '0') * 10 + (correct.charAt(4) - '0') - (current.charAt(3) - '0') * 10 - (current.charAt(4) - '0'), result = minutes / 60 + minutes % 60 / 15 + minutes % 15 / 5 + minutes % 5;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def convertTime(self, current: str, correct: str) -> int:
        return (z := (int(correct[:2]) * 60 + int(correct[3:])) - (int(current[:2]) * 60 + int(current[3:]))) // 60 + z % 60 // 15 + z % 15 // 5 + z % 5
```
