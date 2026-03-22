# 1733 Minimum Number of People to Teach 需要教语言的最少人数

__Description:__

On a social network consisting of `m` users and some friendships between users, two users can communicate with each other if they know a common language.

You are given an integer `n`, an array `languages`, and an array `friendships` where:

- There are `n` languages numbered `1` through `n`,
- `languages[i]` is the set of languages the `i ^ ​​​​​​th`​​​​ user knows, and
- `friendships[i] = [u​​​​​​i​​​, v​​​​​​i]` denotes a friendship between the users `u ^ ​​​​​​​​​​​i`​​​​​ and `vi`.

You can choose one language and teach it to some users so that all friends can communicate with each other. Return the minimum number of users you need to teach.

__Example:__

Example 1:

```text
Input: n = 2, languages = [[1],[2],[1,2]], friendships = [[1,2],[1,3],[2,3]]
Output: 1
Explanation: You can either teach user 1 the second language or user 2 the first language.
```

Example 2:

```text
Input: n = 3, languages = [[2],[1,3],[1,2],[3]], friendships = [[1,4],[1,2],[3,4],[2,3]]
Output: 2
Explanation: Teach the third language to users 1 and 3, yielding two users to teach.
```

__Constraints:__

- `2 <= n <= 500`
- `languages.length == m`
- `1 <= m <= 500`
- `1 <= languages[i].length <= n`
- `1 <= languages[i][j] <= n`
- `1 <= u​​​​​​i < v​​​​​​i <= languages.length`
- `1 <= friendships.length <= 500`
- All tuples `(u​​​​​i, v​​​​​​i)` are unique
- `languages[i]` contains only unique values

__题目描述:__

在一个由 `m` 个用户组成的社交网络里，我们获取到一些用户之间的好友关系。两个用户之间可以相互沟通的条件是他们都掌握同一门语言。

给你一个整数 `n` ，数组 `languages` 和数组 `friendships` ，它们的含义如下:

- 总共有 `n` 种语言，编号从 `1` 到 `n` 。
- `languages[i]` 是第 `i` 位用户掌握的语言集合。
- `friendships[i] = [u​​​​​​i​​​, v​​​​​​i]` 表示 `u ^ ​​​​​​​​​​​i`​​​​​ 和 `vi` 为好友关系。

你可以选择 一门 语言并教会一些用户，使得所有好友之间都可以相互沟通。请返回你 最少 需要教会多少名用户。

__示例:__

示例 1：

```text
输入：n = 2, languages = [[1],[2],[1,2]], friendships = [[1,2],[1,3],[2,3]]
输出：1
解释：你可以选择教用户 1 第二门语言，也可以选择教用户 2 第一门语言。
```

示例 2：

```text
输入：n = 3, languages = [[2],[1,3],[1,2],[3]], friendships = [[1,4],[1,2],[3,4],[2,3]]
输出：2
解释：教用户 1 和用户 3 第三门语言，需要教 2 名用户。
```

__提示：__

- `2 <= n <= 500`
- `languages.length == m`
- `1 <= m <= 500`
- `1 <= languages[i].length <= n`
- `1 <= languages[i][j] <= n`
- `1 <= u​​​​​​i < v​​​​​​i <= languages.length`
- `1 <= friendships.length <= 500`
- 所有的好友关系 `(u​​​​​i, v​​​​​​i)` 都是唯一的。
- `languages[i]` 中包含的值互不相同。

__思路:__

```text
哈希表
使用哈希表记录每一个人会的语言
统计互相不能交流的人加入哈希表
如果没有不能交流的直接返回 0
统计不能交流的人中会的语言最多的
用不能交流的人数减去会的人最多的语言的人数即可
时间复杂度为 O(MN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumTeachings(int n, vector<vector<int>>& languages, vector<vector<int>>& friendships) 
    {
        vector<unordered_set<int>> graph;
        unordered_set<int> need;
        for (const auto& language : languages) graph.emplace_back(unordered_set<int>(language.begin(), language.end()));
        auto check = [&](const auto& s1, const auto& s2) -> bool
        {
            for (const auto& i : s1) if (s2.count(i)) return false;
            return true;
        };
        for (const auto& f : friendships) 
        {
            if (check(graph[f.front() - 1], graph[f.back() - 1])) 
            {
                need.insert(f.front() - 1);
                need.insert(f.back() - 1);
            }
        }
        if (need.empty()) return 0;
        vector<int> c(n);
        for (const auto& p : need) for (int i = 0; i < n; i++) if (graph[p].count(i + 1)) ++c[i];
        return need.size() - *max_element(c.begin(), c.end());
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumTeachings(int n, int[][] languages, int[][] friendships) {
        List<Set<Integer>> graph = new ArrayList<>();
        for (int[] language : languages) graph.add(Arrays.stream(language).boxed().collect(Collectors.toSet()));
        Set<Integer> need = new HashSet<>();
        for (int[] f : friendships) {
            if (check(graph.get(f[0] - 1), graph.get(f[1] - 1))) {
                need.add(f[0] - 1);
                need.add(f[1] - 1);
            }
        }
        if (need.isEmpty()) return 0;
        int[] count = new int[n];
        for (int p : need) for (int i = 0; i < n; i++) if (graph.get(p).contains(i + 1)) ++count[i];
        return need.size() - Arrays.stream(count).boxed().collect(Collectors.toList()).stream().max(Integer::compareTo).get();
    }
    
    private boolean check(Set<Integer> a, Set<Integer> b) {
        for (int j : b) if (a.contains(j)) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution {
    public int minimumTeachings(int n, int[][] languages, int[][] friendships) {
        List<Set<Integer>> graph = new ArrayList<>();
        for (int[] language : languages) graph.add(Arrays.stream(language).boxed().collect(Collectors.toSet()));
        Set<Integer> need = new HashSet<>();
        for (int[] f : friendships) {
            if (check(graph.get(f[0] - 1), graph.get(f[1] - 1))) {
                need.add(f[0] - 1);
                need.add(f[1] - 1);
            }
        }
        if (need.isEmpty()) return 0;
        int[] count = new int[n];
        for (int p : need) for (int i = 0; i < n; i++) if (graph.get(p).contains(i + 1)) ++count[i];
        return need.size() - Arrays.stream(count).boxed().collect(Collectors.toList()).stream().max(Integer::compareTo).get();
    }
    
    private boolean check(Set<Integer> a, Set<Integer> b) {
        for (int j : b) if (a.contains(j)) return false;
        return true;
    }
}
```
