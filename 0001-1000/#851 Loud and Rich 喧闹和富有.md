# 851 Loud and Rich 喧闹和富有

__Description__:
There is a group of n people labeled from 0 to n - 1 where each person has a different amount of money and a different level of quietness.

You are given an array richer where richer[i] = [ai, bi] indicates that ai has more money than bi and an integer array quiet where quiet[i] is the quietness of the ith person. All the given data in richer are logically correct (i.e., the data will not lead you to a situation where x is richer than y and y is richer than x at the same time).

Return an integer array answer where answer[x] = y if y is the least quiet person (that is, the person y with the smallest value of quiet[y]) among all people who definitely have equal to or more money than the person x.

__Example:__

Example 1:

Input: richer = [[1,0],[2,1],[3,1],[3,7],[4,3],[5,3],[6,3]], quiet = [3,2,5,4,6,1,7,0]
Output: [5,5,2,5,4,5,6,7]
Explanation:
answer[0] = 5.
Person 5 has more money than 3, which has more money than 1, which has more money than 0.
The only person who is quieter (has lower quiet[x]) is person 7, but it is not clear if they have more money than person 0.
answer[7] = 7.
Among all people that definitely have equal to or more money than person 7 (which could be persons 3, 4, 5, 6, or 7), the person who is the quietest (has lower quiet[x]) is person 7.
The other answers can be filled out with similar reasoning.

Example 2:

Input: richer = [], quiet = [0]
Output: [0]

__Constraints:__

n == quiet.length
1 <= n <= 500
0 <= quiet[i] < n
All the values of quiet are unique.
0 <= richer.length <= n * (n - 1) / 2
0 <= ai, bi < n
ai != bi
All the pairs of richer are unique.
The observations in richer are all logically consistent.

__题目描述__:
在一组 N 个人（编号为 0, 1, 2, ..., N-1）中，每个人都有不同数目的钱，以及不同程度的安静（quietness）。

为了方便起见，我们将编号为 x 的人简称为 "person x "。

如果能够肯定 person x 比 person y 更有钱的话，我们会说 richer[i] = [x, y] 。注意 richer 可能只是有效观察的一个子集。

另外，如果 person x 的安静程度为 q ，我们会说 quiet[x] = q 。

现在，返回答案 answer ，其中 answer[x] = y 的前提是，在所有拥有的钱不少于 person x 的人中，person y 是最安静的人（也就是安静值 quiet[y] 最小的人）。

__示例 :__

输入：richer = [[1,0],[2,1],[3,1],[3,7],[4,3],[5,3],[6,3]], quiet = [3,2,5,4,6,1,7,0]
输出：[5,5,2,5,4,5,6,7]
解释：
answer[0] = 5，
person 5 比 person 3 有更多的钱，person 3 比 person 1 有更多的钱，person 1 比 person 0 有更多的钱。
唯一较为安静（有较低的安静值 quiet[x]）的人是 person 7，
但是目前还不清楚他是否比 person 0 更有钱。

answer[7] = 7，
在所有拥有的钱肯定不少于 person 7 的人中(这可能包括 person 3，4，5，6 以及 7)，
最安静(有较低安静值 quiet[x])的人是 person 7。

其他的答案也可以用类似的推理来解释。

__提示:__

1 <= quiet.length = N <= 500
0 <= quiet[i] < N，所有 quiet[i] 都不相同。
0 <= richer.length <= N * (N-1) / 2
0 <= richer[i][j] < N
richer[i][0] != richer[i][1]
richer[i] 都是不同的。
对 richer 的观察在逻辑上是一致的。

__思路__:

DFS ➕ 缓存
记录每个人比他富有的人最安静的人
遍历的时候进行剪枝
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> loudAndRich(vector<vector<int>>& richer, vector<int>& quiet) 
    {
        int n = quiet.size();
        vector<int> result(n, -1);
        map<int, vector<int>> m;
        for (const auto& r : richer) m[r.back()].emplace_back(r.front());
        for (int i = 0; i < n; i++) dfs(i, result, m, quiet);
        return result;
    }
private:
    int dfs(int i, vector<int>& result, map<int, vector<int>>& m, vector<int>& quiet) 
    {
        if (result[i] > -1) return result[i];
        result[i] = i;
        for (const auto& j : m[i]) if (quiet[result[i]] > quiet[dfs(j, result, m, quiet)]) result[i] = result[j];
        return result[i];
    }
};
```

__Java__:

```Java
class Solution {
    public int[] loudAndRich(int[][] richer, int[] quiet) {
        int n = quiet.length, result[] = new int[n];
        Map<Integer, List<Integer>> map = new HashMap<>();
        for (int i = 0; i < n; i++) map.put(i, new ArrayList<Integer>());
        for (int[] r : richer) map.get(r[1]).add(r[0]);
        Arrays.fill(result, -1);
        for (int i = 0; i < n; i++) dfs(i, result, map, quiet);
        return result;
    }
    
    private int dfs(int i, int[] result, Map<Integer, List<Integer>> map, int[] quiet) {
        if (result[i] > -1) return result[i];
        result[i] = i;
        for (int j : map.get(i)) if (quiet[result[i]] > quiet[dfs(j, result, map, quiet)]) result[i] = result[j];
        return result[i];
    }
}
```

__Python__:

```Python
class Solution:
    def loudAndRich(self, richer: List[List[int]], quiet: List[int]) -> List[int]:
        graph, result = defaultdict(list), [-1] * (n := len(quiet))
        for x, y in richer:
            graph[y].append(x)
        
        def dfs(person: int) -> int:
            if result[person] >= 0:
                return result[person]
            result[person] = person
            for i in graph[person]:
                if quiet[result[person]] > quiet[dfs(i)]:
                    result[person] = result[i]
            return result[person]
        
        for person in range(n):
            dfs(person)
        return result
```
