# 2092 Find All People With Secret 找出知晓秘密的所有专家

__Description:__

You are given an integer `n` indicating there are `n` people numbered from `0` to `n - 1`. You are also given a __0-indexed__ 2D integer array `meetings` where `meetings[i] = [xi, yi, timei]` indicates that person `xi` and person `yi` have a meeting at `timei`. A person may attend __multiple meetings__ at the same time. Finally, you are given an integer `firstPerson`.

Person `0` has a __secret__ and initially shares the secret with a person `firstPerson` at time `0`. This secret is then shared every time a meeting takes place with a person that has the secret. More formally, for every meeting, if a person `xi` has the secret at `timei`, then they will share the secret with person `yi`, and vice versa.

The secrets are shared instantaneously. That is, a person may receive the secret and share it with people in other meetings within the same time frame.

Return a list of all the people that have the secret after all the meetings have taken place. You may return the answer in any order.

__Example:__

Example 1:

```text
Input: n = 6, meetings = [[1,2,5],[2,3,8],[1,5,10]], firstPerson = 1
Output: [0,1,2,3,5]
Explanation:
At time 0, person 0 shares the secret with person 1.
At time 5, person 1 shares the secret with person 2.
At time 8, person 2 shares the secret with person 3.
At time 10, person 1 shares the secret with person 5.
Thus, people 0, 1, 2, 3, and 5 know the secret after all the meetings.
```

Example 2:

```text
Input: n = 4, meetings = [[3,1,3],[1,2,2],[0,3,3]], firstPerson = 3
Output: [0,1,3]
Explanation:
At time 0, person 0 shares the secret with person 3.
At time 2, neither person 1 nor person 2 know the secret.
At time 3, person 3 shares the secret with person 0 and person 1.
Thus, people 0, 1, and 3 know the secret after all the meetings.
```

Example 3:

```text
Input: n = 5, meetings = [[3,4,2],[1,2,1],[2,3,1]], firstPerson = 1
Output: [0,1,2,3,4]
Explanation:
At time 0, person 0 shares the secret with person 1.
At time 1, person 1 shares the secret with person 2, and person 2 shares the secret with person 3.
Note that person 2 can share the secret at the same time as receiving it.
At time 2, person 3 shares the secret with person 4.
Thus, people 0, 1, 2, 3, and 4 know the secret after all the meetings.
```

__Constraints:__

- `2 <= n <= 10 ^ 5`
- `1 <= meetings.length <= 10 ^ 5`
- `meetings[i].length == 3`
- `0 <= xi, yi <= n - 1`
- `xi != yi`
- `1 <= timei <= 10 ^ 5`
- `1 <= firstPerson <= n - 1`

__题目描述:__

给你一个整数 `n` ，表示有 `n` 个专家从 `0` 到 `n - 1` 编号。另外给你一个下标从 0 开始的二维整数数组 `meetings` ，其中 `meetings[i] = [xi, yi, timei]` 表示专家 `xi` 和专家 `yi` 在时间 `timei` 要开一场会。一个专家可以同时参加 __多场会议__ 。最后，给你一个整数 `firstPerson` 。

专家 `0` 有一个 __秘密__ ，最初，他在时间 `0` 将这个秘密分享给了专家 `firstPerson` 。接着，这个秘密会在每次有知晓这个秘密的专家参加会议时进行传播。更正式的表达是，每次会议，如果专家 `xi` 在时间 `timei` 时知晓这个秘密，那么他将会与专家 `yi` 分享这个秘密，反之亦然。

秘密共享是 瞬时发生 的。也就是说，在同一时间，一个专家不光可以接收到秘密，还能在其他会议上与其他专家分享。

在所有会议都结束之后，返回所有知晓这个秘密的专家列表。你可以按 任何顺序 返回答案。

__示例:__

示例 1：

```text
输入：n = 6, meetings = [[1,2,5],[2,3,8],[1,5,10]], firstPerson = 1
输出：[0,1,2,3,5]
解释：
时间 0 ，专家 0 将秘密与专家 1 共享。
时间 5 ，专家 1 将秘密与专家 2 共享。
时间 8 ，专家 2 将秘密与专家 3 共享。
时间 10 ，专家 1 将秘密与专家 5 共享。
因此，在所有会议结束后，专家 0、1、2、3 和 5 都将知晓这个秘密。
```

示例 2：

```text
输入：n = 4, meetings = [[3,1,3],[1,2,2],[0,3,3]], firstPerson = 3
输出：[0,1,3]
解释：
时间 0 ，专家 0 将秘密与专家 3 共享。
时间 2 ，专家 1 与专家 2 都不知晓这个秘密。
时间 3 ，专家 3 将秘密与专家 0 和专家 1 共享。
因此，在所有会议结束后，专家 0、1 和 3 都将知晓这个秘密。
```

示例 3：

```text
输入：n = 5, meetings = [[3,4,2],[1,2,1],[2,3,1]], firstPerson = 1
输出：[0,1,2,3,4]
解释：
时间 0 ，专家 0 将秘密与专家 1 共享。
时间 1 ，专家 1 将秘密与专家 2 共享，专家 2 将秘密与专家 3 共享。
注意，专家 2 可以在收到秘密的同一时间分享此秘密。
时间 2 ，专家 3 将秘密与专家 4 共享。
因此，在所有会议结束后，专家 0、1、2、3 和 4 都将知晓这个秘密。
```

__提示：__

- `2 <= n <= 10 ^ 5`
- `1 <= meetings.length <= 10 ^ 5`
- `meetings[i].length == 3`
- `0 <= xi, yi <= n - 1`
- `xi != yi`
- `1 <= timei <= 10 ^ 5`
- `1 <= firstPerson <= n - 1`

__思路:__

```text
并查集
将所有时间相同的会议放在一起
先将所有同时间开会的所有人合并
如果合并之后与 0 号专家不在一起
将其父节点重置为自己
时间复杂度为 O(N + MlogM), 空间复杂度为 O(N + M), 其中 M 为 meetings 数组长度
```

__代码:__

__C++__:

```C++
class UnionFind 
{
public:
    vector<int> parent;
    vector<int> size;
    int n;
    int total;

    UnionFind(int _n): n(_n), total(_n), parent(_n), size(_n, 1) 
    {
        iota(parent.begin(), parent.end(), 0);
    }

    int findset(int x) 
    {
        return parent[x] == x ? x : parent[x] = findset(parent[x]);
    }

    bool unite(int x, int y) 
    {
        x = findset(x);
        y = findset(y);
        if ((x = findset(x)) == (y = findset(y))) return false;
        if (size[x] < size[y]) swap(x, y);
        parent[y] = x;
        size[x] += size[y];
        --total;
        return true;
    }

    bool connected(int x, int y) 
    {
        return findset(x) == findset(y);
    }

    void isolate(int x) 
    {
        if (x != parent[x])
        {
            parent[x] = x;
            size[x] = 1;
            ++total;
        }
    }
};

bool cmp(const vector<int>& a, const vector<int>& b)
{
    return a[2] < b[2];
}

class Solution 
{
public:
    vector<int> findAllPeople(int n, vector<vector<int>>& meetings, int firstPerson) 
    {
        sort(meetings.begin(), meetings.end(), cmp);
        UnionFind uf(n);
        uf.unite(firstPerson, 0);
        for (int i = 0, m = meetings.size(); i < m; i++)
        { 
            int j = i + 1;
            while (j < m)
            {
                if (meetings[i][2] != meetings[j][2]) break;
                j++;
            }
            for (int k = i; k < j; k++) uf.unite(meetings[k][0], meetings[k][1]);
            for (int k = i; k < j; k++)
            {
                if (!uf.connected(meetings[k][0], 0))
                {
                    uf.isolate(meetings[k][0]);
                    uf.isolate(meetings[k][1]);
                }
            }
            i = j - 1;
        }
        vector<int> result;
        for (int i = 0; i < n; i++) if (uf.connected(i, 0)) result.emplace_back(i);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private int[] parent;

    private int find(int x) {
        return (parent[x] == x) ? x : (parent[x] = find(parent[x]));
    }
        
    public List<Integer> findAllPeople(int n, int[][] meetings, int firstPerson) {
        parent = new int[n];
        for (int i = 1; i < n; ++ i) parent[i] = i;
        parent[firstPerson] = 0;
        Map<Integer, List<int[]>> map = new TreeMap<>();
        for (int[] m : meetings) {
            List<int[]> list = map.getOrDefault(m[2], new ArrayList<>());
            list.add(m);
            map.put(m[2], list);
        }
        for (int x : map.keySet()) {
            for (int[] l : map.get(x)) {
                int a = l[0], b = l[1];                
                if (parent[find(a)] == 0 || parent[find(b)] == 0) {
                    parent[find(a)] = 0; 
                    parent[find(b)] = 0; 
                }
                parent[find(b)] = parent[find(a)];
            }
            for (int[] l : map.get(x)) {
                int a = l[0], b = l[1];
                if (parent[find(a)] == 0 || parent[find(b)] == 0) { 
                    parent[find(a)] = 0; 
                    parent[find(b)] = 0; 
                } else { 
                    parent[a] = a; 
                    parent[b] = b; 
                }
            }
        }       
        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < n; i++) if (parent[find(i)] == 0) result.add(i);
        return result;
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

    def isolate(self, p: int) -> None:
        self.parent[p] = p


class Solution:
    def findAllPeople(self, n: int, meetings: List[List[int]], firstPerson: int) -> List[int]:
        uf, result = UF(n), [0]
        uf.union(0, firstPerson)
        meetings.sort(key=lambda x: x[2])
        for _, members in groupby(meetings, key=lambda x: x[2]):
            members, p = list(members), set()
            for x, y, _ in members:
                uf.union(x, y)
                p.add(x)
                p.add(y)
            for person in p:
                if uf.find(person) != uf.find(0):
                    uf.isolate(person)
        for i in range(1, n):
            if uf.find(i) == uf.find(0):
                result.append(i)
        return result
```
