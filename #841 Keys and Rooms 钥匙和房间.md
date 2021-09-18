# 841 Keys and Rooms 钥匙和房间

__Description__:
There are n rooms labeled from 0 to n - 1 and all the rooms are locked except for room 0. Your goal is to visit all the rooms. However, you cannot enter a locked room without having its key.

When you visit a room, you may find a set of distinct keys in it. Each key has a number on it, denoting which room it unlocks, and you can take all of them with you to unlock the other rooms.

Given an array rooms where rooms[i] is the set of keys that you can obtain if you visited room i, return true if you can visit all the rooms, or false otherwise.

__Example:__

Example 1:

Input: rooms = [[1],[2],[3],[]]
Output: true
Explanation:
We visit room 0 and pick up key 1.
We then visit room 1 and pick up key 2.
We then visit room 2 and pick up key 3.
We then visit room 3.
Since we were able to visit every room, we return true.

Example 2:

Input: rooms = [[1,3],[3,0,1],[2],[0]]
Output: false
Explanation: We can not enter room number 2 since the only key that unlocks it is in that room.

__Constraints:__

n == rooms.length
2 <= n <= 1000
0 <= rooms[i].length <= 1000
1 <= sum(rooms[i].length) <= 3000
0 <= rooms[i][j] < n
All the values of rooms[i] are unique.

__题目描述__:
有 N 个房间，开始时你位于 0 号房间。每个房间有不同的号码：0，1，2，...，N-1，并且房间里可能有一些钥匙能使你进入下一个房间。

在形式上，对于每个房间 i 都有一个钥匙列表 rooms[i]，每个钥匙 rooms[i][j] 由 [0,1，...，N-1] 中的一个整数表示，其中 N = rooms.length。 钥匙 rooms[i][j] = v 可以打开编号为 v 的房间。

最初，除 0 号房间外的其余所有房间都被锁住。

你可以自由地在房间之间来回走动。

如果能进入每个房间返回 true，否则返回 false。

__示例 :__

示例 1：

输入: [[1],[2],[3],[]]
输出: true
解释:  
我们从 0 号房间开始，拿到钥匙 1。
之后我们去 1 号房间，拿到钥匙 2。
然后我们去 2 号房间，拿到钥匙 3。
最后我们去了 3 号房间。
由于我们能够进入每个房间，我们返回 true。

示例 2：

输入：[[1,3],[3,0,1],[2],[0]]
输出：false
解释：我们不能进入 2 号房间。

__提示:__

1 <= rooms.length <= 1000
0 <= rooms[i].length <= 1000
所有房间中的钥匙数量总计不超过 3000。

__思路__:

BFS
用一个 bool 数组 visited 记录访问过的位置
所有位置都被访问则是可以用钥匙打开
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    void dfs(int key, const vector<vector<int>>& rooms, vector<bool>& visited) 
    {
        if (visited[key]) return;
        visited[key] = true;
        for (int key : rooms[key]) dfs(key, rooms, visited);
    }
public:
    bool canVisitAllRooms(vector<vector<int>>& rooms) 
    {
        vector<bool> visited(rooms.size());
        dfs(0, rooms, visited);
        for (int i : visited) if (!i) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canVisitAllRooms(List<List<Integer>> rooms) {
        boolean visited[] = new boolean[rooms.size()];
        dfs(0, rooms, visited);
        for (boolean i : visited) if (!i) return false;
        return true;
    }
    
    private void dfs(int key, List<List<Integer>> rooms, boolean[] visited) {
        if (visited[key]) return;
        visited[key] = true;
        for (int keys : rooms.get(key)) dfs(keys, rooms, visited);
    }
}
```

__Python__:

```Python
class Solution:
    def canVisitAllRooms(self, rooms: List[List[int]]) -> bool:
        visited = [False] * len(rooms)
        
        def dfs(i: int) -> None:
            if visited[i]:
                return
            visited[i] = True
            for key in rooms[i]:
                if not visited[key]:
                    dfs(key)
        dfs(0)
        return all(visited)
```
