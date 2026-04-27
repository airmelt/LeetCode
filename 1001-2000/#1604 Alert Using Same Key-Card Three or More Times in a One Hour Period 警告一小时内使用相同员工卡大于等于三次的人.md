# 1604 Alert Using Same Key-Card Three or More Times in a One Hour Period 警告一小时内使用相同员工卡大于等于三次的人

__Description:__

LeetCode company workers use key-cards to unlock office doors. Each time a worker uses their key-card, the security system saves the worker's name and the time when it was used. The system emits an alert if any worker uses the key-card three or more times in a one-hour period.

You are given a list of strings `keyName` and `keyTime` where `[keyName[i], keyTime[i]]` corresponds to a person's name and the time when their key-card was used __in a__ __single day__.

Access times are given in the __24-hour time format "HH:MM"__, such as `"23:51"` and `"09:49"`.

Return a list of unique worker names who received an alert for frequent keycard use. Sort the names in ascending order alphabetically.

Notice that `"10:00"` - `"11:00"` is considered to be within a one-hour period, while `"22:51"` - `"23:52"` is not considered to be within a one-hour period.

__Example:__

Example 1:

```text
Input: keyName = ["daniel","daniel","daniel","luis","luis","luis","luis"], keyTime = ["10:00","10:40","11:00","09:00","11:00","13:00","15:00"]
Output: ["daniel"]
Explanation: "daniel" used the keycard 3 times in a one-hour period ("10:00","10:40", "11:00").
```

Example 2:

```text
Input: keyName = ["alice","alice","alice","bob","bob","bob","bob"], keyTime = ["12:01","12:00","18:00","21:00","21:20","21:30","23:00"]
Output: ["bob"]
Explanation: "bob" used the keycard 3 times in a one-hour period ("21:00","21:20", "21:30").
```

__Constraints:__

- `1 <= keyName.length, keyTime.length <= 10 ^ 5`
- `keyName.length == keyTime.length`
- `keyTime[i]` is in the format __"HH:MM"__.
- `[keyName[i], keyTime[i]]` is __unique__.
- `1 <= keyName[i].length <= 10`
- `keyName[i] contains only lowercase English letters.`

__题目描述:__

力扣公司的员工都使用员工卡来开办公室的门。每当一个员工使用一次他的员工卡，安保系统会记录下员工的名字和使用时间。如果一个员工在一小时时间内使用员工卡的次数大于等于三次，这个系统会自动发布一个 警告 。

给你字符串数组 `keyName` 和 `keyTime` ，其中 `[keyName[i], keyTime[i]]` 对应一个人的名字和他在 __某一天__ 内使用员工卡的时间。

使用时间的格式是 __24小时制__ ，形如 __"HH:MM"__ ，比方说 `"23:51"` 和 `"09:49"` 。

请你返回去重后的收到系统警告的员工名字，将它们按 字典序升序 排序后返回。

请注意 `"10:00"` - `"11:00"` 视为一个小时时间范围内，而 `"22:51"` - `"23:52"` 不被视为一小时时间范围内。

__示例:__

示例 1：

```text
输入：keyName = ["daniel","daniel","daniel","luis","luis","luis","luis"], keyTime = ["10:00","10:40","11:00","09:00","11:00","13:00","15:00"]
输出：["daniel"]
解释："daniel" 在一小时内使用了 3 次员工卡（"10:00"，"10:40"，"11:00"）。
```

示例 2：

```text
输入：keyName = ["alice","alice","alice","bob","bob","bob","bob"], keyTime = ["12:01","12:00","18:00","21:00","21:20","21:30","23:00"]
输出：["bob"]
解释："bob" 在一小时内使用了 3 次员工卡（"21:00"，"21:20"，"21:30"）。
```

__提示：__

- `1 <= keyName.length, keyTime.length <= 10 ^ 5`
- `keyName.length == keyTime.length`
- `keyTime` 格式为 __"HH:MM"__ 。
- 保证 `[keyName[i], keyTime[i]]` 形成的二元对 __互不相同__ 。
- `1 <= keyName[i].length <= 10`
- `keyName[i]` 只包含小写英文字母。

__思路:__

```text
模拟
将时间按照分钟数插入哈希表
按名字遍历哈希表, 只要有违反打卡时间的人就加入结果
最后将结果排序
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<string> alertNames(vector<string>& keyName, vector<string>& keyTime) 
    {
        map<string, vector<int>> m;
        vector<string> result;
        for (int i = 0, n = keyName.size(); i < n; i++) m[keyName[i]].emplace_back(((keyTime[i][0] - '0') * 10 + (keyTime[i][1] - '0')) * 60 + (keyTime[i][3] - '0') * 10 + (keyTime[i][4] - '0'));
        for (auto& [k, v] : m)
        {
            sort(v.begin(), v.end());
            for (int i = 2, sz = v.size(); i < sz; i++)
            {
                if (v[i] - v[i - 2] <= 60)
                {
                    result.emplace_back(k);
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
    public List<String> alertNames(String[] keyName, String[] keyTime) {
        Map<String, List<Integer>> map = new HashMap<>();
        List<String> result = new ArrayList<>();
        for (int i = 0, n = keyName.length; i < n; i++) {
            List<Integer> cur = map.getOrDefault(keyName[i], new ArrayList<>());
            String[] times = keyTime[i].split(":");
            cur.add(Integer.valueOf(times[0]) * 60 + Integer.valueOf(times[1]));
            map.put(keyName[i], cur);
        }
        for (Map.Entry<String, List<Integer>> entry : map.entrySet()) {
            List<Integer> timeList = entry.getValue();
            Collections.sort(timeList);
            for (int i = 2, m = timeList.size(); i < m; i++) {
                if (timeList.get(i) - timeList.get(i - 2) <= 60) {
                    result.add(entry.getKey());
                    break;
                }
            }
        }
        Collections.sort(result);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def alertNames(self, keyName: List[str], keyTime: List[str]) -> List[str]:
        times, result = defaultdict(list), []
        for name, time in zip(keyName, keyTime):
            times[name].append(int(time[:2]) * 60 + int(time[3:]))
        for name, t in times.items():
            t.sort()
            if any(t2 - t1 <= 60 for t1, t2 in zip(t, t[2:])):
                result.append(name)
        result.sort()
        return result
```
