# 1817 Finding the Users Active Minutes 查找用户活跃分钟数

__Description:__

You are given the logs for users' actions on LeetCode, and an integer `k`. The logs are represented by a 2D integer array `logs` where each `logs[i] = [IDi, timei]` indicates that the user with `IDi` performed an action at the minute `timei`.

Multiple users can perform actions simultaneously, and a single user can perform multiple actions in the same minute.

The user active minutes (UAM) for a given user is defined as the number of unique minutes in which the user performed an action on LeetCode. A minute can only be counted once, even if multiple actions occur during it.

You are to calculate a __1-indexed__ array `answer` of size `k` such that, for each `j` ( `1 <= j <= k`), `answer[j]` is the __number of users__ whose __UAM__ equals `j`.

Return _the array_ `answer` _as described above_.

__Example:__

Example 1:

```text
Input: logs = [[0,5],[1,2],[0,2],[0,5],[1,3]], k = 5
Output: [0,2,0,0,0]
Explanation:
The user with ID=0 performed actions at minutes 5, 2, and 5 again. Hence, they have a UAM of 2 (minute 5 is only counted once).
The user with ID=1 performed actions at minutes 2 and 3. Hence, they have a UAM of 2.
Since both users have a UAM of 2, answer[2] is 2, and the remaining answer[j] values are 0.
```

Example 2:

```text
Input: logs = [[1,1],[2,2],[2,3]], k = 4
Output: [1,1,0,0]
Explanation:
The user with ID=1 performed a single action at minute 1. Hence, they have a UAM of 1.
The user with ID=2 performed actions at minutes 2 and 3. Hence, they have a UAM of 2.
There is one user with a UAM of 1 and one with a UAM of 2.
Hence, answer[1] = 1, answer[2] = 1, and the remaining values are 0.
```

__Constraints:__

- `1 <= logs.length <= 10 ^ 4`
- `0 <= IDi <= 10 ^ 9`
- `1 <= timei <= 10 ^ 5`
- `k` is in the range `[The maximum __UAM__ for a user, 10 ^ 5]`.

__题目描述:__

给你用户在 LeetCode 的操作日志，和一个整数 `k` 。日志用一个二维整数数组 `logs` 表示，其中每个 `logs[i] = [IDi, timei]` 表示 ID 为 `IDi` 的用户在 `timei` 分钟时执行了某个操作。

多个用户 可以同时执行操作，单个用户可以在同一分钟内执行 多个操作 。

指定用户的 用户活跃分钟数（user active minutes，UAM） 定义为用户对 LeetCode 执行操作的 唯一分钟数 。 即使一分钟内执行多个操作，也只能按一分钟计数。

请你统计用户活跃分钟数的分布情况，统计结果是一个长度为 `k` 且 __下标从 1 开始计数__ 的数组 `answer` ，对于每个 `j`（ `1 <= j <= k`）， `answer[j]` 表示 __用户活跃分钟数__ 等于 `j` 的用户数。

返回上面描述的答案数组 __`answer`__。

__示例:__

示例 1：

```text
输入：logs = [[0,5],[1,2],[0,2],[0,5],[1,3]], k = 5
输出：[0,2,0,0,0]
解释：
ID=0 的用户执行操作的分钟分别是：5 、2 和 5 。因此，该用户的用户活跃分钟数为 2（分钟 5 只计数一次）
ID=1 的用户执行操作的分钟分别是：2 和 3 。因此，该用户的用户活跃分钟数为 2
2 个用户的用户活跃分钟数都是 2 ，answer[2] 为 2 ，其余 answer[j] 的值都是 0
```

示例 2：

```text
输入：logs = [[1,1],[2,2],[2,3]], k = 4
输出：[1,1,0,0]
解释：
ID=1 的用户仅在分钟 1 执行单个操作。因此，该用户的用户活跃分钟数为 1
ID=2 的用户执行操作的分钟分别是：2 和 3 。因此，该用户的用户活跃分钟数为 2
1 个用户的用户活跃分钟数是 1 ，1 个用户的用户活跃分钟数是 2 
因此，answer[1] = 1 ，answer[2] = 1 ，其余的值都是 0
```

__提示：__

- `1 <= logs.length <= 10 ^ 4`
- `0 <= IDi <= 10 ^ 9`
- `1 <= timei <= 10 ^ 5`
- `k` 的取值范围是 `[用户的最大用户活跃分钟数, 10 ^ 5]`

__思路:__

```text
1. 哈希表
将所有 logs 都插入到哈希表中, key 为用户 ID, value 为用户活跃分钟数的集合
遍历哈希表, 将用户活跃分钟数的集合的大小作为下标, 将对应的值减一加入结果数组中
时间复杂度为 O(N + K), 空间复杂度为 O(N)
2. 排序
将 logs 按照用户 ID 和时间进行排序
遍历 logs 列表, 统计每个用户的用户活跃分钟数
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> findingUsersActiveMinutes(vector<vector<int>>& logs, int k) 
    {
        sort(logs.begin(), logs.end());
        int id = logs.front().front(), time = 1, n = logs.size();
        vector<int> result(k);
        for (int i = 1; i < n; i++) 
        {
            if (logs[i].front() == id and logs[i].back() != logs[i - 1].back()) ++time;
            else if (logs[i].front() != id) 
            {
                ++result[time - 1];
                id = logs[i].front();
                time = 1;
            }
        }
        ++result[time - 1];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] findingUsersActiveMinutes(int[][] logs, int k) {
        Arrays.sort(logs, (l1, l2) -> l1[0] == l2[0] ? l1[1] - l2[1] : l1[0] - l2[0]);
        int id = logs[0][0], time = 1, result[] = new int[k], n = logs.length;
        for (int i = 1; i < n; i++) {
            if (logs[i][0] == id && logs[i][1] != logs[i - 1][1]) ++time;
            else if (logs[i][0] != id) {
                ++result[time - 1];
                id = logs[i][0];
                time = 1;
            }
        }
        ++result[time - 1];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findingUsersActiveMinutes(self, logs: List[List[int]], k: int) -> List[int]:
        d = defaultdict(set)
        for log in logs:
            d[log[0]].add(log[1])
        result = [0] * k
        for i in d.values():
            result[len(i) - 1] += 1
        return result
```
