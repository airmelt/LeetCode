# 1409 Queries on a Permutation With Key 查询带键的排列

__Description:__

Given the array `queries` of positive integers between `1` and `m`, you have to process all `queries[i]` (from `i=0` to `i=queries.length-1`) according to the following rules:

- In the beginning, you have the permutation `P=[1,2,3,...,m]`.
- For the current `i`, find the position of `queries[i]` in the permutation `P` (__indexing from 0__) and then move this at the beginning of the permutation `P.` Notice that the position of `queries[i]` in `P` is the result for `queries[i]`.

Return an array containing the result for the given `queries`.

__Example:__

Example 1:

```text
Input: queries = [3,1,2,1], m = 5
Output: [2,1,2,1] 
Explanation: The queries are processed as follow: 
For i=0: queries[i]=3, P=[1,2,3,4,5], position of 3 in P is 2, then we move 3 to the beginning of P resulting in P=[3,1,2,4,5]. 
For i=1: queries[i]=1, P=[3,1,2,4,5], position of 1 in P is 1, then we move 1 to the beginning of P resulting in P=[1,3,2,4,5]. 
For i=2: queries[i]=2, P=[1,3,2,4,5], position of 2 in P is 2, then we move 2 to the beginning of P resulting in P=[2,1,3,4,5]. 
For i=3: queries[i]=1, P=[2,1,3,4,5], position of 1 in P is 1, then we move 1 to the beginning of P resulting in P=[1,2,3,4,5]. 
Therefore, the array containing the result is [2,1,2,1].
```

Example 2:

```text
Input: queries = [4,1,2,2], m = 4
Output: [3,1,2,0]
```

Example 3:

```text
Input: queries = [7,5,5,8,3], m = 8
Output: [6,5,0,7,5]
```

__Constraints:__

- `1 <= m <= 10 ^ 3`
- `1 <= queries.length <= m`
- `1 <= queries[i] <= m`

__题目描述:__

给你一个待查数组 `queries` ，数组中的元素为 `1` 到 `m` 之间的正整数。 请你根据以下规则处理所有待查项 `queries[i]`（从 `i=0` 到 `i=queries.length-1`）：

- 一开始，排列 `P=[1,2,3,...,m]`。
- 对于当前的 `i` ，请你找出待查项 `queries[i]` 在排列 `P` 中的位置（__下标从 0 开始__），然后将其从原位置移动到排列 `P` 的起始位置（即下标为 0 处）。注意， `queries[i]` 在 `P` 中的位置就是 `queries[i]` 的查询结果。

请你以数组形式返回待查数组  `queries` 的查询结果。

__示例:__

示例 1：

```text
输入：queries = [3,1,2,1], m = 5
输出：[2,1,2,1] 
解释：待查数组 queries 处理如下：
对于 i=0: queries[i]=3, P=[1,2,3,4,5], 3 在 P 中的位置是 2，接着我们把 3 移动到 P 的起始位置，得到 P=[3,1,2,4,5] 。
对于 i=1: queries[i]=1, P=[3,1,2,4,5], 1 在 P 中的位置是 1，接着我们把 1 移动到 P 的起始位置，得到 P=[1,3,2,4,5] 。 
对于 i=2: queries[i]=2, P=[1,3,2,4,5], 2 在 P 中的位置是 2，接着我们把 2 移动到 P 的起始位置，得到 P=[2,1,3,4,5] 。
对于 i=3: queries[i]=1, P=[2,1,3,4,5], 1 在 P 中的位置是 1，接着我们把 1 移动到 P 的起始位置，得到 P=[1,2,3,4,5] 。 
因此，返回的结果数组为 [2,1,2,1] 。
```

示例 2：

```text
输入：queries = [4,1,2,2], m = 4
输出：[3,1,2,0]
```

示例 3：

```text
输入：queries = [7,5,5,8,3], m = 8
输出：[6,5,0,7,5]
```

__提示：__

- `1 <= m <= 10 ^ 3`
- `1 <= queries.length <= m`
- `1 <= queries[i] <= m`

__思路:__

```text
1. 模拟
由于数据范围比较小
可以尝试暴力模拟
每次找到数字对应下标删除后放到第一个位置即可
删除和插入数字的时间复杂度为 O(M)
存放数字需要 O(M) 的空间
时间复杂度为 O(MN), 空间复杂度为 O(M)
2. 树状数组
假设数组前有足够的空间存放数字
可以将数字找到之后放置在数组开头
那样每次只需要查找数字的位置即可
可以用 (M + N) 的空间来预留数字的位置
将数字移动可以用单点修改的方式完成
查询位置就是查询这个位置前有多少个数字
有数字的用 1 表示, 没数字的用 0 表示
时间复杂度为 O(Nlog(M + N)), 空间复杂度为 O(M + N)
```

__代码:__

__C++__:

```C++
struct BIT 
{
    vector<int> a;
    int n;
    
    BIT(int _n): n(_n), a(_n + 1) {}
    
    static int lowbit(int x) 
    {
        return x & (-x);
    }

    int query(int x) const 
    {
        int result = 0;
        while (x) 
        {
            result += a[x];
            x -= lowbit(x);
        }
        return result;
    }

    void update(int x, int v) 
    {
        while (x <= n) {
            a[x] += v;
            x += lowbit(x);
        }
    }
};

class Solution 
{
public:
    vector<int> processQueries(vector<int>& queries, int m) 
    {
        int n = queries.size();
        BIT bit(m + n);
        vector<int> pos(m + 1);
        for (int i = 1; i <= m; ++i) 
        {
            pos[i] = n + i;
            bit.update(n + i, 1);
        }
        vector<int> result;
        for (int i = 0; i < n; ++i) 
        {
            int& cur = pos[queries[i]];
            bit.update(cur, -1);
            result.emplace_back(bit.query(cur));
            cur = n - i;
            bit.update(cur, 1);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] processQueries(int[] queries, int m) {
        List<Integer> p = new ArrayList<Integer>();
        for (int i = 1; i <= m; i++) p.add(i);
        int n = queries.length, result[] = new int[n];
        for (int i = 0; i < n; i++) {
            int query = queries[i], pos = -1;
            for (int j = 0; j < m; j++) if (p.get(j) == query) pos = j;
            result[i] = pos;
            p.remove(pos);
            p.add(0, query);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def processQueries(self, queries: List[int], m: int) -> List[int]:
        p, result = deque([i + 1 for i in range(m)]), []
        for query in queries:
            result.append(pos := p.index(query))
            del p[pos]
            p.appendleft(query)
        return result
```
