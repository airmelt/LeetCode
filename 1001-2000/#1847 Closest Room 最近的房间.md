# 1847 Closest Room 最近的房间

__Description:__

There is a hotel with `n` rooms. The rooms are represented by a 2D integer array `rooms` where `rooms[i] = [roomIdi, sizei]` denotes that there is a room with room number `roomIdi` and size equal to `sizei`. Each `roomIdi` is guaranteed to be __unique__.

You are also given `k` queries in a 2D array `queries` where `queries[j] = [preferredj, minSizej]`. The answer to the `j ^ th` query is the room number `id` of a room such that:

- The room has a size of __at least__ `minSizej`, and
- `abs(id - preferredj)` is __minimized__, where `abs(x)` is the absolute value of `x`.

If there is a __tie__ in the absolute difference, then use the room with the __smallest__ such `id`. If there is __no such room__, the answer is `-1`.

Return _an array_ `answer` _of length_ `k` _where_ `answer[j]` _contains the answer to the_ `j ^ th` _query_.

__Example:__

Example 1:

```text
Input: rooms = [[2,2],[1,2],[3,2]], queries = [[3,1],[3,3],[5,2]]
Output: [3,-1,3]
Explanation: The answers to the queries are as follows:
Query = [3,1]: Room number 3 is the closest as abs(3 - 3) = 0, and its size of 2 is at least 1. The answer is 3.
Query = [3,3]: There are no rooms with a size of at least 3, so the answer is -1.
Query = [5,2]: Room number 3 is the closest as abs(3 - 5) = 2, and its size of 2 is at least 2. The answer is 3.
```

Example 2:

```text
Input: rooms = [[1,4],[2,3],[3,5],[4,1],[5,2]], queries = [[2,3],[2,4],[2,5]]
Output: [2,1,3]
Explanation: The answers to the queries are as follows:
Query = [2,3]: Room number 2 is the closest as abs(2 - 2) = 0, and its size of 3 is at least 3. The answer is 2.
Query = [2,4]: Room numbers 1 and 3 both have sizes of at least 4. The answer is 1 since it is smaller.
Query = [2,5]: Room number 3 is the only room with a size of at least 5. The answer is 3.
```

__Constraints:__

- `n == rooms.length`
- `1 <= n <= 10 ^ 5`
- `k == queries.length`
- `1 <= k <= 10 ^ 4`
- `1 <= roomIdi, preferredj <= 10 ^ 7`
- `1 <= sizei, minSizej <= 10 ^ 7`

__题目描述:__

一个酒店里有 `n` 个房间，这些房间用二维整数数组 `rooms` 表示，其中 `rooms[i] = [roomIdi, sizei]` 表示有一个房间号为 `roomIdi` 的房间且它的面积为 `sizei` 。每一个房间号 `roomIdi` 保证是 __独一无二__ 的。

同时给你 `k` 个查询，用二维数组 `queries` 表示，其中 `queries[j] = [preferredj, minSizej]` 。第 `j` 个查询的答案是满足如下条件的房间 `id` :

- 房间的面积 _至少_ 为 `minSizej` ，且
- `abs(id - preferredj)` 的值 __最小__ ，其中 `abs(x)` 是 `x` 的绝对值。

如果差的绝对值有 __相等__ 的，选择 __最小__ 的 `id` 。如果 __没有满足条件的房间__ ，答案为 `-1` 。

请你返回长度为 `k` 的数组 `answer` ，其中 `answer[j]` 为第 `j` 个查询的结果。

__示例:__

示例 1：

```text
输入：rooms = [[2,2],[1,2],[3,2]], queries = [[3,1],[3,3],[5,2]]
输出：[3,-1,3]
解释：查询的答案如下：
查询 [3,1] ：房间 3 的面积为 2 ，大于等于 1 ，且号码是最接近 3 的，为 abs(3 - 3) = 0 ，所以答案为 3 。
查询 [3,3] ：没有房间的面积至少为 3 ，所以答案为 -1 。
查询 [5,2] ：房间 3 的面积为 2 ，大于等于 2 ，且号码是最接近 5 的，为 abs(3 - 5) = 2 ，所以答案为 3 。
```

示例 2：

```text
输入：rooms = [[1,4],[2,3],[3,5],[4,1],[5,2]], queries = [[2,3],[2,4],[2,5]]
输出：[2,1,3]
解释：查询的答案如下：
查询 [2,3] ：房间 2 的面积为 3 ，大于等于 3 ，且号码是最接近的，为 abs(2 - 2) = 0 ，所以答案为 2 。
查询 [2,4] ：房间 1 和 3 的面积都至少为 4 ，答案为 1 因为它房间编号更小。
查询 [2,5] ：房间 3 是唯一面积大于等于 5 的，所以答案为 3 。
```

__提示：__

- `n == rooms.length`
- `1 <= n <= 10 ^ 5`
- `k == queries.length`
- `1 <= k <= 10 ^ 4`
- `1 <= roomIdi, preferredj <= 10 ^ 7`
- `1 <= sizei, minSizej <= 10 ^ 7`

__思路:__

```text
离线 ➕ 二分查找
将房间和查询看作事件，将房间的事件和查询的事件合并按照面积从大到小排序
遍历事件
如果是房间就将房间加入有序集合/列表中
如果是查询就在有序集合中二分查找
查询到最接近的房间, 比较两边房间的序号, 选择合适的房间
时间复杂度为 O((M + N)log(M + N)), 空间复杂度为 O(M + N), 其中 M 为房间数, N 为查询数
```

__代码:__

__C++__:

```C++
struct Event 
{
    int op;
    int size;
    int idx;
    int origin;
    
    Event(int _op, int _size, int _idx, int _origin): op(_op), size(_size), idx(_idx), origin(_origin) {}

    bool operator< (const Event& other) const 
    {
        return size > other.size or (size == other.size and op < other.op);
    }
};

class Solution 
{
public:
    vector<int> closestRoom(vector<vector<int>>& rooms, vector<vector<int>>& queries) 
    {
        int m = rooms.size(), n = queries.size();
        vector<Event> events;
        for (int i = 0; i < m; i++) events.emplace_back(0, rooms[i][1], rooms[i][0], i);
        for (int i = 0; i < n; i++) events.emplace_back(1, queries[i][1], queries[i][0], i);
        sort(events.begin(), events.end());
        vector<int> result(n, -1);
        set<int> valid;
        for (const auto& event: events) 
        {
            if (!event.op) valid.insert(event.idx);
            else 
            {
                int dist = INT_MAX;
                auto it = valid.lower_bound(event.idx);
                if (it != valid.end() and *it - event.idx < dist) 
                {
                    dist = *it - event.idx;
                    result[event.origin] = *it;
                }
                if (it != valid.begin()) 
                {
                    it = prev(it);
                    if (event.idx - *it <= dist) 
                    {
                        dist = event.idx - *it;
                        result[event.origin] = *it;
                    }
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
    public int[] closestRoom(int[][] rooms, int[][] queries) {
        Arrays.sort(rooms, (a, b) -> a[1] - b[1]);
        int n = queries.length, idQueries[][] = new int[n][3], result[] = new int[n], cur = rooms.length - 1;
        for (int i = 0; i < queries.length; i++) {
            idQueries[i][0] = queries[i][0];
            idQueries[i][1] = queries[i][1];
            idQueries[i][2] = i;
        }
        Arrays.sort(idQueries, (a, b) -> a[1] - b[1]);
        Arrays.fill(result, -1);
        TreeSet<Integer> set = new TreeSet<>();
        set.add(1000_000_000);
        set.add(-1000_000_000);
        for (int i = n - 1; i > -1; i--) {
            while (cur >= 0 && rooms[cur][1] >= idQueries[i][1]) set.add(rooms[cur--][0]);
            int high = set.ceiling(idQueries[i][0]), low = set.floor(idQueries[i][0]);
            if (low < 0 && high == 1000_000_000) continue;
            if (high - idQueries[i][0] < idQueries[i][0] - low) result[idQueries[i][2]] = high;
            else result[idQueries[i][2]] = low;
        }
        return result;
    }
}
```

__Python__:

```Python
import sortedcontainers
class Event:
    def __init__(self, op: int, size: int, idx: int, origin: int):
        self.op = op
        self.size = size
        self.idx = idx
        self.origin = origin

    def __lt__(self, other: "Event") -> bool:
        return self.size > other.size or (self.size == other.size and self.op < other.op)


class Solution:
    def closestRoom(self, rooms: List[List[int]], queries: List[List[int]]) -> List[int]:
        events, result, valid = sorted([Event(0, size, room_id, i) for i, (room_id, size) in enumerate(rooms)] + [Event(1, prefer, size, i) for i, (size, prefer) in enumerate(queries)]), [-1] * (n := len(queries)), sortedcontainers.SortedList()
        for event in events:
            if not event.op:
                valid.add(event.idx)
            else:
                dist, x = inf, valid.bisect_left(event.idx)
                if x != len(valid) and valid[x] - event.idx < dist:
                    dist, result[event.origin] = valid[x] - event.idx, valid[x]
                if x:
                    if event.idx - valid[x := x - 1] <= dist:
                        dist, result[event.origin] = event.idx - valid[x], valid[x]
        return result
```
