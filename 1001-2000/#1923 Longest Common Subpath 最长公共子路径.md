# 1923 Longest Common Subpath 最长公共子路径

__Description:__

There is a country of `n` cities numbered from `0` to `n - 1`. In this country, there is a road connecting _every pair_ of cities.

There are `m` friends numbered from `0` to `m - 1` who are traveling through the country. Each one of them will take a path consisting of some cities. Each path is represented by an integer array that contains the visited cities in order. The path may contain a city __more than once__, but the same city will not be listed consecutively.

Given an integer `n` and a 2D integer array `paths` where `paths[i]` is an integer array representing the path of the `i ^ th` friend, return _the length of the __longest common subpath__ that is shared by __every__ friend's path, or_ `0` _if there is no common subpath at all_.

A subpath of a path is a contiguous sequence of cities within that path.

__Example:__

Example 1:

```text
Input: n = 5, paths = [[0,1,2,3,4],
                       [2,3,4],
                       [4,0,1,2,3]]
Output: 2
Explanation: The longest common subpath is [2,3].
```

Example 2:

```text
Input: n = 3, paths = [[0],[1],[2]]
Output: 0
Explanation: There is no common subpath shared by the three paths.
```

Example 3:

```text
Input: n = 5, paths = [[0,1,2,3,4],
                       [4,3,2,1,0]]
Output: 1
Explanation: The possible longest common subpaths are [0], [1], [2], [3], and [4]. All have a length of 1.
```

__Constraints:__

- `1 <= n <= 10 ^ 5`
- `m == paths.length`
- `2 <= m <= 10 ^ 5`
- `sum(paths[i].length) <= 10 ^ 5`
- `0 <= paths[i][j] < n`
- The same city is not listed multiple times consecutively in `paths[i]`.

__题目描述:__

一个国家由 `n` 个编号为 `0` 到 `n - 1` 的城市组成。在这个国家里，__每两个__ 城市之间都有一条道路连接。

总共有 `m` 个编号为 `0` 到 `m - 1` 的朋友想在这个国家旅游。他们每一个人的路径都会包含一些城市。每条路径都由一个整数数组表示，每个整数数组表示一个朋友按顺序访问过的城市序列。同一个城市在一条路径中可能 __重复__ 出现，但同一个城市在一条路径中不会连续出现。

给你一个整数 `n` 和二维数组 `paths` ，其中 `paths[i]` 是一个整数数组，表示第 `i` 个朋友走过的路径，请你返回 __每一个__ 朋友都走过的 __最长公共子路径__ 的长度，如果不存在公共子路径，请你返回 `0` 。

一个 子路径 指的是一条路径中连续的城市序列。

__示例:__

示例 1：

```text
输入：n = 5, paths = [[0,1,2,3,4],
                     [2,3,4],
                     [4,0,1,2,3]]
输出：2
解释：最长公共子路径为 [2,3] 。
```

示例 2：

```text
输入：n = 3, paths = [[0],[1],[2]]
输出：0
解释：三条路径没有公共子路径。
```

示例 3：

```text
输入：n = 5, paths = [[0,1,2,3,4],
                     [4,3,2,1,0]]
输出：1
解释：最长公共子路径为 [0]，[1]，[2]，[3] 和 [4] 。它们长度都为 1 。
```

__提示：__

- `1 <= n <= 10 ^ 5`
- `m == paths.length`
- `2 <= m <= 10 ^ 5`
- `sum(paths[i].length) <= 10 ^ 5`
- `0 <= paths[i][j] < n`
- `paths[i]` 中同一个城市不会连续重复出现。

__思路:__

```text
注意到长度一定是单调的, 也就是说如果 result 满足题意, 那么 [1, result] 中的任意整数一定都满足题意
所以我们可以从 [1, min(len(path) for path in paths)] 区间中进行二分查找找到最长的公共子路径
1. 二分查找 ➕ 哈希
使用滚动哈希 (Rabin-Karp) 算法将子数组压缩成一个整数并放入集合中
首先将第 0 个子数组中所有长度为 mid 的子数组的哈希值放入集合中
然后遍历剩下的子数组, 将其哈希值与集合中的哈希值进行比较, 如果存在相同的哈希值, 则说明存在相同的子数组
这个算法并不能 100% 保证不发生哈希冲突
时间复杂度为 O(NlogN), 空间复杂度为 O(N), 其中 N 为 paths 数组中所有子数组的总长度
2. 后缀数组
用一个分隔符将子数组转化成字符串
将所有子数组连接成一个总路径
如果一个路径为所有公共子路径, 那么它在总路径中至少出现 m 次
需要比较 sa[i] 和 sa[i + 1] 的公共前缀
如果有 m 个公共前缀的长度都不小于 mid, 说明 mid 是一个可能的最长公共子路径
时间复杂度为 O(NlogN), 空间复杂度为 O(N), 其中 N 为 paths 数组中所有子数组的总长度
```

__代码:__

__C++__:

```C++
// 后缀数组，生成sa，rk，height数组
template<class T=string, int range=128>
struct SuffixArray
{
    T s;
    int n, bucket_range;
    int sa[range], second[range], bucket[range], mem[range], rk_mem[range + 1], rk2_mem[range + 1], height[range], *rk, *rk2;
    SuffixArray(const T&_s): s(_s), n(s.size()), bucket_range(range)
    {
        rk = rk_mem;
        rk2 = rk2_mem;
        rk[n] = rk2[n] = -1;
        memset(bucket, 0, sizeof(bucket));
        for (int i = 0; i < n; i++) ++bucket[rk[i] = s[i]];
        for (int i = 1; i < bucket_range; i++) bucket[i] += bucket[i - 1];
        for (int i = 0; i < n; i++) sa[--bucket[rk[i]]] = i;
        for (int w = 1; ; w <<= 1)
        {
            int j = 0;
            for (int i = n - w; i < n; i++) second[j++] = i;
            for (int i = 0; i < n; i++) if (sa[i] >= w) second[j++] = sa[i] - w;
            memset(bucket, 0, sizeof(bucket));
            for (int i = 0; i < n; i++) bucket[mem[i] = rk[second[i]]]++;
            for (int i = 1; i < bucket_range; i++) bucket[i] += bucket[i - 1];
            for (int i = n - 1; i >= 0; i--) sa[--bucket[mem[i]]] = second[i];
            bucket_range = 0;
            for (int i = 0; i < n; i++) 
            {
                rk2[sa[i]] = !i or (rk[sa[i]] == rk[sa[i - 1]] and rk[sa[i] + w] == rk[sa[i - 1] + w]) ? bucket_range : ++bucket_range;
            }
            swap(rk, rk2);
            if (++bucket_range == n) break;
        }
    }

    void getHeight()
    {
        memset(height, 0xff, sizeof(height));
        for (int i = 0, h = 0; i < n; i++) 
        {
            if (h) --h;
            if (rk[i]) while (sa[rk[i] - 1] + h < n and s[i + h] == s[sa[rk[i] - 1] + h]) h++;
            height[rk[i]] = h;
        }
    }
};
class Solution 
{
public:
    int longestCommonSubpath(int n, vector<vector<int>>& paths) 
    {
        int m = paths.size();
        vector<int> all, sublen, belong;
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < paths[i].size(); j++) 
            {
                all.push_back(paths[i][j]);
                sublen.push_back(paths[i].size() - j);
                belong.push_back(i);
            }
        }
        SuffixArray<vector<int>, 100000>SA(all);
        SA.getHeight();
        auto sa = SA.sa, h = SA.height;
        int k = all.size(), left = 0, right = k;
        while (left < right)
        {
            int mid = (left + right + 1) >> 1, flag = false;
            for (int i = 0, j; i < k; i = j)
            {
                counter.reset();
                for (j = i; j < k and (j == i or h[j] >= mid); j++) if (sublen[sa[j]] >= mid) counter.add(belong[sa[j]]);
                if (counter.num == m)
                {
                    flag = true;
                    break;
                }
            }
            if (flag) left = mid;
            else right = mid - 1;
        }
        return left;
    }
private:
    struct
    {
        int mark[100000] = {0}, num = 0;
        int timestamp = 0;
        void reset()
        {
            timestamp++;
            num = 0;
        }

        void add(int i)
        {
            if (mark[i] != timestamp)
            {
                mark[i] = timestamp;
                ++num;
            }
        }
    } counter;
};
```

__Java__:

```Java
class Solution {
    private HashSet<Long> all;
    private int[][] paths;
    private int i;
    private long hash;

    public int longestCommonSubpath(int n, int[][] paths) {
        if (paths.length == 1) return paths[0].length;
        int left = 1, right = paths[0].length, result = 0;
        this.paths = paths;
        for (int[] c : paths) right = Math.min(right, c.length);
        while (left <= right) {
            int mid = (left + right) >> 1;
            if (check(mid)) {
                left = mid + 1;
                result = Math.max(result, mid);
            } else {
                right = mid - 1;
            }
            all = null;
        }
        return result;
    }

    private boolean check(int m) {
        int cur = 0, left = 0;
        all = new HashSet();
        hash = 0;
        for (i = 0; i < m; i++) {
            hash += (paths[0][i] + 13) * (i + 1);
            cur += paths[0][i];
        }
        all.add(hash * (paths[0][i - 1] + 3) + paths[0][i - 1] % 31);
        while (i < paths[0].length) {
            hash = hash - cur + paths[0][i] * m;
            cur -= paths[0][left++];
            cur += paths[0][i];
            all.add(hash * (paths[0][i] + 3) + paths[0][i] % 31);
            ++i;
        }
        for (int j = 1; j < paths.length; j++) {
            left = 0;
            cur = 0;
            HashSet<Long> all1 = new HashSet();
            hash = 0;
            for (i = 0; i < m; i++) {
                hash += (paths[j][i] + 13) * (i + 1);
                cur += paths[j][i];
            }
            long a = hash * (paths[j][i - 1] + 3) + paths[j][i - 1] % 31;
            if (all.contains(a)) all1.add(a);
            while (i < paths[j].length) {
                hash = hash - cur + paths[j][i] * m;
                cur -= paths[j][left++];
                cur += paths[j][i];
                a = hash * (paths[j][i] + 3) + paths[j][i] % 31;
                if (all.contains(a)) all1.add(a);
                ++i;
            }
            if (all1.size() == 0) return false;
            all = all1;
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def longestCommonSubpath(self, n: int, paths: List[List[int]]) -> int:
        MOD, base, m, left, right, result = (10 ** 9 + 7) * (10 ** 9 + 9), random.randint(10 ** 6, 10 ** 7), len(paths), 1, len(min(paths, key=lambda p: len(p))), 0
        while left <= right:
            v, s, check = pow(base, (mid := (left + right) >> 1), MOD), set(), True
            for i in range(m):
                hash_value, t = 0, set()
                for j in range(mid):
                    hash_value = (hash_value * base + paths[i][j]) % MOD
                if not i or hash_value in s:
                    t.add(hash_value)
                for j in range(mid, len(paths[i])):
                    hash_value = (hash_value * base - paths[i][j - mid] * v + paths[i][j]) % MOD
                    if i == 0 or hash_value in s:
                        t.add(hash_value)
                if not t:
                    check = False
                    break
                s = t
            if check:
                result, left = mid, mid + 1
            else:
                right = mid - 1
        return result
```
