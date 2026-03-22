# 2076 Process Restricted Friend Requests 处理含限制条件的好友请求

__Description:__

You are given an integer `n` indicating the number of people in a network. Each person is labeled from `0` to `n - 1`.

You are also given a __0-indexed__ 2D integer array `restrictions`, where `restrictions[i] = [xi, yi]` means that person `xi` and person `yi` __cannot__ become __friends__, either __directly__ or __indirectly__ through other people.

Initially, no one is friends with each other. You are given a list of friend requests as a __0-indexed__ 2D integer array `requests`, where `requests[j] = [uj, vj]` is a friend request between person `uj` and person `vj`.

A friend request is __successful__ if `uj` and `vj` can be __friends__. Each friend request is processed in the given order (i.e., `requests[j]` occurs before `requests[j + 1]`), and upon a successful request, `uj` and `vj` __become direct friends__ for all future friend requests.

Return _a __boolean array___ `result`, _where each_ `result[j]` _is_ `true` _if the_ `j ^ th` _friend request is __successful__ or_ `false` _if it is not_.

__Note:__ If `uj` and `vj` are already direct friends, the request is still __successful__.

__Example:__

Example 1:

```text
Input: n = 3, restrictions = [[0,1]], requests = [[0,2],[2,1]]
Output: [true,false]
Explanation:
Request 0: Person 0 and person 2 can be friends, so they become direct friends. 
Request 1: Person 2 and person 1 cannot be friends since person 0 and person 1 would be indirect friends (1--2--0).
```

Example 2:

```text
Input: n = 3, restrictions = [[0,1]], requests = [[1,2],[0,2]]
Output: [true,false]
Explanation:
Request 0: Person 1 and person 2 can be friends, so they become direct friends.
Request 1: Person 0 and person 2 cannot be friends since person 0 and person 1 would be indirect friends (0--2--1).
```

Example 3:

```text
Input: n = 5, restrictions = [[0,1],[1,2],[2,3]], requests = [[0,4],[1,2],[3,1],[3,4]]
Output: [true,false,true,false]
Explanation:
Request 0: Person 0 and person 4 can be friends, so they become direct friends.
Request 1: Person 1 and person 2 cannot be friends since they are directly restricted.
Request 2: Person 3 and person 1 can be friends, so they become direct friends.
Request 3: Person 3 and person 4 cannot be friends since person 0 and person 1 would be indirect friends (0--4--3--1).
```

__Constraints:__

- `2 <= n <= 1000`
- `0 <= restrictions.length <= 1000`
- `restrictions[i].length == 2`
- `0 <= xi, yi <= n - 1`
- `xi != yi`
- `1 <= requests.length <= 1000`
- `requests[j].length == 2`
- `0 <= uj, vj <= n - 1`
- `uj != vj`

__题目描述:__

给你一个整数 `n` ，表示网络上的用户数目。每个用户按从 `0` 到 `n - 1` 进行编号。

给你一个下标从 __0__ 开始的二维整数数组 `restrictions` ，其中 `restrictions[i] = [xi, yi]` 意味着用户 `xi` 和用户 `yi` __不能__ 成为 __朋友__ ，不管是 __直接__ 还是通过其他用户 __间接__ 。

最初，用户里没有人是其他用户的朋友。给你一个下标从 __0__ 开始的二维整数数组 `requests` 表示好友请求的列表，其中 `requests[j] = [uj, vj]` 是用户 `uj` 和用户 `vj` 之间的一条好友请求。

如果 `uj` 和 `vj` 可以成为 __朋友__ ，那么好友请求将会 __成功__ 。每个好友请求都会按列表中给出的顺序进行处理（即， `requests[j]` 会在 `requests[j + 1]` 前）。一旦请求成功，那么对所有未来的好友请求而言， `uj` 和 `vj` 将会 __成为直接朋友 。__

返回一个 __布尔数组__ `result` ，其中元素遵循此规则:如果第 `j` 个好友请求 __成功__ ，那么 `result[j]` 就是 `true` ；否则，为 `false` 。

__注意:__如果 `uj` 和 `vj` 已经是直接朋友，那么他们之间的请求将仍然 __成功__ 。

__示例:__

示例 1：

```text
输入：n = 3, restrictions = [[0,1]], requests = [[0,2],[2,1]]
输出：[true,false]
解释：
请求 0 ：用户 0 和 用户 2 可以成为朋友，所以他们成为直接朋友。 
请求 1 ：用户 2 和 用户 1 不能成为朋友，因为这会使 用户 0 和 用户 1 成为间接朋友 (1--2--0) 。
```

示例 2：

```text
输入：n = 3, restrictions = [[0,1]], requests = [[1,2],[0,2]]
输出：[true,false]
解释：
请求 0 ：用户 1 和 用户 2 可以成为朋友，所以他们成为直接朋友。 
请求 1 ：用户 0 和 用户 2 不能成为朋友，因为这会使 用户 0 和 用户 1 成为间接朋友 (0--2--1) 。
```

示例 3：

```text
输入：n = 5, restrictions = [[0,1],[1,2],[2,3]], requests = [[0,4],[1,2],[3,1],[3,4]]
输出：[true,false,true,false]
解释：
请求 0 ：用户 0 和 用户 4 可以成为朋友，所以他们成为直接朋友。 
请求 1 ：用户 1 和 用户 2 不能成为朋友，因为他们之间存在限制。
请求 2 ：用户 3 和 用户 1 可以成为朋友，所以他们成为直接朋友。 
请求 3 ：用户 3 和 用户 4 不能成为朋友，因为这会使 用户 0 和 用户 1 成为间接朋友 (0--4--3--1) 。
```

__提示：__

- `2 <= n <= 1000`
- `0 <= restrictions.length <= 1000`
- `restrictions[i].length == 2`
- `0 <= xi, yi <= n - 1`
- `xi != yi`
- `1 <= requests.length <= 1000`
- `requests[j].length == 2`
- `0 <= uj, vj <= n - 1`
- `uj != vj`

__思路:__

```text
并查集
将可以成为朋友的加入并查集
遍历请求, 如果已经是朋友则返回 true, 即两个节点在同一个分量
否则检查是否有限制条件
遍历限制条件, 如果两个节点在限制条件中, 则返回 false
即 (x, y) = (a, b) or (x, y) = (b, a), a, b 为限制条件中的根节点
否则将两个节点加入并查集并返回 true
时间复杂度为 O(MN), 空间复杂度为 O(N), 其中 N 为 n, M 为 requests 数组的长度
```

__代码:__

__C++__:

```C++
class UF 
{   
public:
    UF(int n): _count(n), parent(n), weight(n, 1) 
    {
        iota(parent.begin(), parent.end(), 0);
    }
    
    int find(int p) 
    {
        return parent[p] == p ? p : parent[p] = find(parent[p]);
    }
    
    void unite(int p, int q) 
    {
        if ((p = find(p)) == (q = find(q))) return;
        if (weight[p] > weight[q]) swap(p, q);
        parent[q] = p;
        weight[p] += weight[q];
        --_count;
    }
    
    bool connected(int p, int q) 
    {    
        return find(p) == find(q);
    }
private:
    vector<int> parent;
    vector<int> weight;
    int _count;
};

class Solution {
public:
    vector<bool> friendRequests(int n, vector<vector<int>>& restrictions, vector<vector<int>>& requests) {
        UF uf(n);
        vector<bool> result;
        for (const auto& request: requests) 
        {
            int x = uf.find(request.front()), y = uf.find(request.back());
            if (x != y) 
            {
                bool check = true;
                for (const auto& restriction: restrictions) 
                {
                    int u = uf.find(restriction.front()), v = uf.find(restriction.back());
                    if ((x == u and y == v) or (x == v and y == u)) 
                    {
                        check = false;
                        break;
                    }
                }
                if (check) uf.unite(x, y);
                result.emplace_back(check);
            }
            else result.emplace_back(true);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean[] friendRequests(int n, int[][] restrictions, int[][] requests) {
        UF uf = new UF(n);
        boolean[] result = new boolean[requests.length];
        for (int i = 0, m = requests.length; i < m; i++) {
            int x = uf.find(requests[i][0]), y = uf.find(requests[i][1]);
            if (x != y) {
                boolean check = true;
                for (int[] restriction: restrictions) {
                    int u = uf.find(restriction[0]), v = uf.find(restriction[1]);
                    if ((x == u && y == v) || (x == v && y == u)) {
                        check = false;
                        break;
                    }
                }
                if (check) uf.unite(x, y);
                result[i] = check;
            }
            else result[i] = true;
        }
        return result;
    }

    class UF { 
        private int[] parent, weight;
        private int count;

        UF(int n) {
            parent = new int[n];
            for (int i = 0; i < n; i++) parent[i] = i;
            weight = new int[n];
            Arrays.fill(weight, 1);
            count = n;
        }
    
        public int find(int p) {
            return (parent[p] == p) ? p : (parent[p] = find(parent[p]));
        }
    
        public void unite(int p, int q) {
            if ((p = find(p)) == (q = find(q))) return;
            if (weight[q] > weight[p]) {
                parent[q] = p;
                weight[p] += weight[q];
            } else {
                parent[p] = q;
                weight[q] += weight[p];
            }
            --count;
        }
    
        public boolean connected(int p, int q) {    
            return find(p) == find(q);
        }
    }
}
```

__Python__:

```Python
class UF:
    def __init__(self, n: int) -> None:
        self.parent = [i for i in range(n)]
        self.weight = [1] * n
        self.count = n

    def union(self, p: int, q: int) -> None:
        """
        连接两个点
        :param p: 一个节点
        :param q: 另一个节点
        :return:
        """
        p = self.find(p)
        q = self.find(q)
        if p == q:
            return
        if self.weight[p] > self.weight[q]:
            self.parent[q] = p
            self.weight[p] += self.weight[q]
        else:
            self.parent[p] = q
            self.weight[q] += self.weight[p]
        self.count -= 1

    def connected(self, p: int, q: int) -> bool:
        """
        检查两个点是否在同一分量
        :param p: 一个节点
        :param q: 另一个节点
        :return: 返回两个点是否在同一个分量
        """
        return self.find(p) == self.find(q)

    def find(self, p: int) -> int:
        """
        查找根节点, 并进行路径压缩
        :param p: 一个节点
        :return: 根节点
        """
        while self.parent[p] != p:
            self.parent[p] = self.parent[self.parent[p]]
            p = self.parent[p]
        return p

class Solution:
    def friendRequests(self, n: int, restrictions: List[List[int]], requests: List[List[int]]) -> List[bool]:
        uf, result = UF(n), [True] * len(requests)
        for i, (u, v) in enumerate(requests):
            if (x := uf.find(u)) != (y := uf.find(v)):
                if all((uf.find(a), uf.find(b)) != (x, y) and (uf.find(a), uf.find(b)) != (y, x) for a, b in restrictions):
                    uf.union(x, y)
                else:
                    result[i] = False
        return result
```
