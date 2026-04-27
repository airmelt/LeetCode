# 1815 Maximum Number of Groups Getting Fresh Donuts 得到新鲜甜甜圈的最多组数

__Description:__

There is a donuts shop that bakes donuts in batches of `batchSize`. They have a rule where they must serve __all__ of the donuts of a batch before serving any donuts of the next batch. You are given an integer `batchSize` and an integer array `groups`, where `groups[i]` denotes that there is a group of `groups[i]` customers that will visit the shop. Each customer will get exactly one donut.

When a group visits the shop, all customers of the group must be served before serving any of the following groups. A group will be happy if they all get fresh donuts. That is, the first customer of the group does not receive a donut that was left over from the previous group.

You can freely rearrange the ordering of the groups. Return the maximum possible number of happy groups after rearranging the groups.

__Example:__

Example 1:

```text
Input: batchSize = 3, groups = [1,2,3,4,5,6]
Output: 4
Explanation: You can arrange the groups as [6,2,4,5,1,3]. Then the 1st, 2nd, 4th, and 6th groups will be happy.
```

Example 2:

```text
Input: batchSize = 4, groups = [1,3,2,5,2,2,1,6]
Output: 4
```

__Constraints:__

- `1 <= batchSize <= 9`
- `1 <= groups.length <= 30`
- `1 <= groups[i] <= 10 ^ 9`

__题目描述:__

有一个甜甜圈商店，每批次都烤 `batchSize` 个甜甜圈。这个店铺有个规则，就是在烤一批新的甜甜圈时，之前 __所有__ 甜甜圈都必须已经全部销售完毕。给你一个整数 `batchSize` 和一个整数数组 `groups` ，数组中的每个整数都代表一批前来购买甜甜圈的顾客，其中 `groups[i]` 表示这一批顾客的人数。每一位顾客都恰好只要一个甜甜圈。

当有一批顾客来到商店时，他们所有人都必须在下一批顾客来之前购买完甜甜圈。如果一批顾客中第一位顾客得到的甜甜圈不是上一组剩下的，那么这一组人都会很开心。

你可以随意安排每批顾客到来的顺序。请你返回在此前提下，最多 有多少组人会感到开心。

__示例:__

示例 1：

```text
输入：batchSize = 3, groups = [1,2,3,4,5,6]
输出：4
解释：你可以将这些批次的顾客顺序安排为 [6,2,4,5,1,3] 。那么第 1，2，4，6 组都会感到开心。
```

示例 2：

```text
输入：batchSize = 4, groups = [1,3,2,5,2,2,1,6]
输出：4
```

__提示：__

- `1 <= batchSize <= 9`
- `1 <= groups.length <= 30`
- `1 <= groups[i] <= 10 ^ 9`

__思路:__

```text
状态压缩 ➕ 动态规划
买甜甜圈的个数与答案无关, 只有取余的值与答案有关
如果取余结果为 0, 则这一组人一定会开心
如果取余结果能够配对, 则这两组组人一定有一组会开心
对剩下的所有人进行遍历
可以使用状态压缩
由于人数小于 30, 可以使用 5 位二进制数表示每一组人的人数
使用一个 20 位的二进制数表示剩下的所有人
30 / 9 = 4, 使用一个 4 位的二进制数表示当前剩下的甜甜圈的数量
每次当甜甜圈的数量为 0 时, 结果加 1, 并记录结果的最大值
时间复杂度为 O(N ^ 2 * 2 ^ N), 空间复杂度为 O(N * 2 ^ N), N 为 batchSize
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxHappyGroups(int batchSize, vector<int> &groups) 
    {
        m = batchSize;
        int result = 0, mask = 0;
        vector<int> cnt(m);
        for (const auto& g : groups) 
        {
            int x = g % m;
            if (!x) ++result;
            else if (cnt[m - x])
            {
                --cnt[m - x];
                ++result;
            } 
            else ++cnt[x];
        }
        for (int x = 1; x < m; x++)
        {
            if (cnt[x]) 
            {
                val.push_back(x);
                mask = mask << 5 | cnt[x];
            }
        }
        reverse(val.begin(), val.end());
        return result + dfs(mask);
    }
private:
    int m;
    vector<int> val;
    unordered_map<int, int> cache;

    int dfs(int mask) 
    {
        if (cache.count(mask)) return cache[mask];
        int r = 0, left = mask >> 20, cur = mask & ((1 << 20) - 1), n = val.size();
        for (int i = 0, j = 0; i < val.size(); i++, j += 5) if (cur >> j & 31) r = max(r, (!left) + dfs((left + val[i]) % m << 20 | cur - (1 << j)));
        return cache[mask] = r;
    }
};
```

__Java__:

```Java
class Solution {
    private int m;
    private int[] val;
    private final Map<Integer, Integer> cache = new HashMap<>();

    public int maxHappyGroups(int batchSize, int[] groups) {
        m = batchSize;
        int result = 0, count[] = new int[m], mask = 0, n = 0;
        for (int g : groups) {
            int x = g % m;
            if (x == 0) ++result;
            else if (count[m - x] > 0) {
                --count[m - x];
                ++result;
            } else ++count[x];
        }
        for (int c : count) if (c > 0) ++n;
        val = new int[n];
        for (int x = 1; x < m; ++x)
            if (count[x] > 0) {
                val[--n] = x;
                mask = mask << 5 | count[x];
            }

        return result + dfs(mask);
    }

    private int dfs(int mask) {
        if (cache.containsKey(mask)) return cache.get(mask);
        int r = 0, left = mask >> 20, msk = mask & ((1 << 20) - 1);
        for (int i = 0, j = 0; i < val.length; ++i, j += 5) if ((msk >> j & 31) > 0) r = Math.max(r, (left == 0 ? 1 : 0) + dfs((left + val[i]) % m << 20 | msk - (1 << j)));
        cache.put(mask, r);
        return r;
    }
}
```

__Python__:

```Python
class Solution:
    def maxHappyGroups(self, batchSize: int, groups: List[int]) -> int:
        result, count = 0, [0] * batchSize
        for g in groups:
            if not (x := g % batchSize):
                result += 1
            elif count[-x]:
                count[-x] -= 1
                result += 1
            else:
                count[x] += 1

        @lru_cache(None)
        def dfs(left: int, cur: Tuple[int]) -> int:
            r, c = 0, list(cur)
            for i, x in enumerate(c):
                if x:
                    c[i] -= 1
                    r = max(r, (not left) + dfs((left + i + 1) % batchSize, tuple(c)))
                    c[i] += 1
            return r
        return result + dfs(0, tuple(count[1:]))
```
