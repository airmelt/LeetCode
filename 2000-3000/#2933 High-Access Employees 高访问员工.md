# 2933 High-Access Employees 高访问员工

__Description:__

You are given a 2D __0-indexed__ array of strings, `access_times`, with size `n`. For each `i` where `0 <= i <= n - 1`, `access_times[i][0]` represents the name of an employee, and `access_times[i][1]` represents the access time of that employee. All entries in `access_times` are within the same day.

The access time is represented as __four digits__ using a __24-hour__ time format, for example, `"0800"` or `"2250"`.

An employee is said to be high-access if he has accessed the system three or more times within a one-hour period.

Times with exactly one hour of difference are __not__ considered part of the same one-hour period. For example, `"0815"` and `"0915"` are not part of the same one-hour period.

Access times at the start and end of the day are __not__ counted within the same one-hour period. For example, `"0005"` and `"2350"` are not part of the same one-hour period.

Return a list that contains the names of high-access employees with any order you want.

__Example:__

Example 1:

```text
Input: access_times = [["a","0549"],["b","0457"],["a","0532"],["a","0621"],["b","0540"]]
Output: ["a"]
Explanation: "a" has three access times in the one-hour period of [05:32, 06:31] which are 05:32, 05:49, and 06:21.
But "b" does not have more than two access times at all.
So the answer is ["a"].
```

Example 2:

```text
Input: access_times = [["d","0002"],["c","0808"],["c","0829"],["e","0215"],["d","1508"],["d","1444"],["d","1410"],["c","0809"]]
Output: ["c","d"]
Explanation: "c" has three access times in the one-hour period of [08:08, 09:07] which are 08:08, 08:09, and 08:29.
"d" has also three access times in the one-hour period of [14:10, 15:09] which are 14:10, 14:44, and 15:08.
However, "e" has just one access time, so it can not be in the answer and the final answer is ["c","d"].
```

Example 3:

```text
Input: access_times = [["cd","1025"],["ab","1025"],["cd","1046"],["cd","1055"],["ab","1124"],["ab","1120"]]
Output: ["ab","cd"]
Explanation: "ab" has three access times in the one-hour period of [10:25, 11:24] which are 10:25, 11:20, and 11:24.
"cd" has also three access times in the one-hour period of [10:25, 11:24] which are 10:25, 10:46, and 10:55.
So the answer is ["ab","cd"].
```

__Constraints:__

- `1 <= access_times.length <= 100`
- `access_times[i].length == 2`
- `1 <= access_times[i][0].length <= 10`
- `access_times[i][0]` consists only of English small letters.
- `access_times[i][1].length == 4`
- `access_times[i][1]` is in 24-hour time format.
- `access_times[i][1]` consists only of `'0'` to `'9'`.

__题目描述:__

给你一个长度为 `n` 、下标从 __0__ 开始的二维字符串数组 `access_times` 。对于每个 `i`（ `0 <= i <= n - 1` ）， `access_times[i][0]` 表示某位员工的姓名， `access_times[i][1]` 表示该员工的访问时间。 `access_times` 中的所有条目都发生在同一天内。

访问时间用 __四位__ 数字表示， 符合 __24 小时制__ ，例如 `"0800"` 或 `"2250"` 。

如果员工在 同一小时内 访问系统 三次或更多 ，则称其为 高访问 员工。

时间间隔正好相差一小时的时间 __不__ 被视为同一小时内。例如， `"0815"` 和 `"0915"` 不属于同一小时内。

一天开始和结束时的访问时间不被计算为同一小时内。例如， `"0005"` 和 `"2350"` 不属于同一小时内。

以列表形式，按任意顺序，返回所有 高访问 员工的姓名。

__示例:__

示例 1：

```text
输入：access_times = [["a","0549"],["b","0457"],["a","0532"],["a","0621"],["b","0540"]]
输出：["a"]
解释："a" 在时间段 [05:32, 06:31] 内有三条访问记录，时间分别为 05:32 、05:49 和 06:21 。
但是 "b" 的访问记录只有两条。
因此，答案是 ["a"] 。
```

示例 2：

```text
输入：access_times = [["d","0002"],["c","0808"],["c","0829"],["e","0215"],["d","1508"],["d","1444"],["d","1410"],["c","0809"]]
输出：["c","d"]
解释："c" 在时间段 [08:08, 09:07] 内有三条访问记录，时间分别为 08:08 、08:09 和 08:29 。
"d" 在时间段 [14:10, 15:09] 内有三条访问记录，时间分别为 14:10 、14:44 和 15:08 。
然而，"e" 只有一条访问记录，因此不能包含在答案中，最终答案是 ["c","d"] 。
```

示例 3：

```text
输入：access_times = [["cd","1025"],["ab","1025"],["cd","1046"],["cd","1055"],["ab","1124"],["ab","1120"]]
输出：["ab","cd"]
解释："ab"在时间段 [10:25, 11:24] 内有三条访问记录，时间分别为 10:25 、11:20 和 11:24 。
"cd" 在时间段 [10:25, 11:24] 内有三条访问记录，时间分别为 10:25 、10:46 和 10:55 。
因此，答案是 ["ab","cd"] 。
```

__提示：__

- `1 <= access_times.length <= 100`
- `access_times[i].length == 2`
- `1 <= access_times[i][0].length <= 10`
- `access_times[i][0]` 仅由小写英文字母组成。
- `access_times[i][1].length == 4`
- `access_times[i][1]` 采用24小时制表示时间。
- `access_times[i][1]` 仅由数字 `'0'` 到 `'9'` 组成。

__思路:__

```text
哈希表
将同一个名字的放在一个组里
将字符串转化为分钟数存入哈希表
对每一个组进行排序
如果相邻 3 个的距离不超过 60 就提前退出将用户加入结果
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<string> findHighAccessEmployees(vector<vector<string>>& access_times) 
    {
        map<string, vector<int>> m;
        for (const auto& access_time: access_times) m[access_time.front()].emplace_back(stoi(access_time.back().substr(0, 2)) * 60 + stoi(access_time.back().substr(2)));
        vector<string> result;
        for (auto& [user, access_time]: m) 
        {
            sort(access_time.begin(), access_time.end());
            for (int i = 2; i < access_time.size(); i++) 
            {
                if (access_time[i] - access_time[i - 2] < 60) 
                {
                    result.emplace_back(user);
                    break;
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> findHighAccessEmployees(List<List<String>> access_times) {
        var map = new HashMap<String, List<Integer>>();
        for (var accessTime : access_times) map.computeIfAbsent(accessTime.get(0), k -> new ArrayList<>()).add(Integer.parseInt(accessTime.get(1).substring(0, 2)) * 60 + Integer.parseInt(accessTime.get(1).substring(2)));
        var result = new ArrayList<String>();
        for (var entry : map.entrySet()) {
            Collections.sort(entry.getValue());
            for (int i = 2; i < entry.getValue().size(); i++) {
                if (entry.getValue().get(i) - entry.getValue().get(i - 2) < 60) {
                    result.add(entry.getKey());
                    break;
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findHighAccessEmployees(self, access_times: List[List[str]]) -> List[str]:
        d, result = defaultdict(list), []
        for user, times in access_times:
            d[user].append(int(times[:2]) * 60 + int(times[2:]))
        for user, times_list in d.items():
            times_list.sort()
            if any(times_list[i] - times_list[i - 2] < 60 for i in range(2, len(times_list))):
                result.append(user)
        return result
```
