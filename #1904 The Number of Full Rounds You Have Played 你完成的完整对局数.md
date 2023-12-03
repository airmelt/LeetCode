# 1904 The Number of Full Rounds You Have Played 你完成的完整对局数

__Description:__

You are participating in an online chess tournament. There is a chess round that starts every `15` minutes. The first round of the day starts at `00:00`, and after every `15` minutes, a new round starts.

- For example, the second round starts at `00:15`, the fourth round starts at `00:45`, and the seventh round starts at `01:30`.

You are given two strings `loginTime` and `logoutTime` where:

- `loginTime` is the time you will login to the game, and
- `logoutTime` is the time you will logout from the game.

If `logoutTime` is __earlier__ than `loginTime`, this means you have played from `loginTime` to midnight and from midnight to `logoutTime`.

Return the number of full chess rounds you have played in the tournament.

__Note:__ All the given times follow the 24-hour clock. That means the first round of the day starts at `00:00` and the last round of the day starts at `23:45`.

__Example:__

Example 1:

```text
Input: loginTime = "09:31", logoutTime = "10:14"
Output: 1
Explanation: You played one full round from 09:45 to 10:00.
You did not play the full round from 09:30 to 09:45 because you logged in at 09:31 after it began.
You did not play the full round from 10:00 to 10:15 because you logged out at 10:14 before it ended.
```

Example 2:

```text
Input: loginTime = "21:30", logoutTime = "03:00"
Output: 22
Explanation: You played 10 full rounds from 21:30 to 00:00 and 12 full rounds from 00:00 to 03:00.
10 + 12 = 22.
```

__Constraints:__

- `loginTime` and `logoutTime` are in the format `hh:mm`.
- `00 <= hh <= 23`
- `00 <= mm <= 59`
- `loginTime` and `logoutTime` are not equal.

__题目描述:__

一款新的在线电子游戏在近期发布，在该电子游戏中，以 __刻钟__ 为周期规划若干时长为 __15 分钟__ 的游戏对局。这意味着，在 `HH:00`、 `HH:15`、 `HH:30` 和 `HH:45` ，将会开始一个新的对局，其中 `HH` 用一个从 `00` 到 `23` 的整数表示。游戏中使用 __24 小时制的时钟__ ，所以一天中最早的时间是 `00:00` ，最晚的时间是 `23:59` 。

给你两个字符串 `startTime` 和 `finishTime` ，均符合 `"HH:MM"` 格式，分别表示你 __进入__ 和 __退出__ 游戏的确切时间，请计算在整个游戏会话期间，你完成的 __完整对局的对局数__ 。

- 例如，如果 `startTime = "05:20"` 且 `finishTime = "05:59"` ，这意味着你仅仅完成从 `05:30` 到 `05:45` 这一个完整对局。而你没有完成从 `05:15` 到 `05:30` 的完整对局，因为你是在对局开始后进入的游戏；同时，你也没有完成从 `05:45` 到 `06:00` 的完整对局，因为你是在对局结束前退出的游戏。

如果 `finishTime` __早于__ `startTime` ，这表示你玩了个通宵（也就是从 `startTime` 到午夜，再从午夜到 `finishTime`）。

假设你是从 `startTime` 进入游戏，并在 `finishTime` 退出游戏，请计算并返回你完成的 __完整对局的对局数__ 。

__示例:__

示例 1：

```text
输入：startTime = "12:01", finishTime = "12:44"
输出：1
解释：你完成了从 12:15 到 12:30 的一个完整对局。
你没有完成从 12:00 到 12:15 的完整对局，因为你是在对局开始后的 12:01 进入的游戏。
你没有完成从 12:30 到 12:45 的完整对局，因为你是在对局结束前的 12:44 退出的游戏。
```

示例 2：

```text
输入：startTime = "20:00", finishTime = "06:00"
输出：40
解释：你完成了从 20:00 到 00:00 的 16 个完整的对局，以及从 00:00 到 06:00 的 24 个完整的对局。
16 + 24 = 40
```

示例 3：

```text
输入：startTime = "00:00", finishTime = "23:59"
输出：95
解释：除最后一个小时你只完成了 3 个完整对局外，其余每个小时均完成了 4 场完整对局。
```

__提示：__

- `startTime` 和 `finishTime` 的格式为 `HH:MM`
- `00 <= HH <= 23`
- `00 <= MM <= 59`
- `startTime` 和 `finishTime` 不相等

__思路:__

```text
模拟
转化为分钟或者小时加时间
如果结束时间比开始时间早, 说明少了一天需要加上 24 小时
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfRounds(string loginTime, string logoutTime) 
    {
        return (((logoutTime < loginTime) * 24) + stoi(logoutTime.substr(0, 2)) - stoi(loginTime.substr(0, 2)) - 1) * 4 + (60 - stoi(loginTime.substr(3))) / 15 + stoi(logoutTime.substr(3)) / 15 > 0 ? (((logoutTime < loginTime) * 24) + stoi(logoutTime.substr(0, 2)) - stoi(loginTime.substr(0, 2)) - 1) * 4 + (60 - stoi(loginTime.substr(3))) / 15 + stoi(logoutTime.substr(3)) / 15 : 0;
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfRounds(String loginTime, String logoutTime) {
        int t1 = ((loginTime.charAt(0) - '0') * 10 + (loginTime.charAt(1) - '0')) * 60 + (loginTime.charAt(3) - '0') * 10 + (loginTime.charAt(4) - '0'), t2 = ((logoutTime.charAt(0) - '0') * 10 + (logoutTime.charAt(1) - '0')) * 60 + (logoutTime.charAt(3) - '0') * 10 + (logoutTime.charAt(4) - '0');
        if (t1 > t2) t2 += 1440;
        return Math.max(t2 / 15 - (t1 + 14) / 15, 0);
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfRounds(self, loginTime: str, logoutTime: str) -> int:
        return result if (result := (((logoutTime < loginTime) * 24) + int(logoutTime[:2]) - int(loginTime[:2]) - 1) * 4 + (60 - int(loginTime[3:])) // 15 + int(logoutTime[3:]) // 15) > 0 else 0
```
