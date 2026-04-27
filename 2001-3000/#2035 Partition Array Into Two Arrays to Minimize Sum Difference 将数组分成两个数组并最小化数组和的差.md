# 2035 Partition Array Into Two Arrays to Minimize Sum Difference 将数组分成两个数组并最小化数组和的差

__Description:__

You are given an integer array `nums` of `2 * n` integers. You need to partition `nums` into __two__ arrays of length `n` to __minimize the absolute difference__ of the __sums__ of the arrays. To partition `nums`, put each element of `nums` into __one__ of the two arrays.

Return the minimum possible absolute difference.

__Example:__

Example 1:

![2035-1](https://assets.leetcode.com/uploads/2021/10/02/ex1.png)

```text
Input: nums = [3,9,7,3]
Output: 2
Explanation: One optimal partition is: [3,9] and [7,3].
The absolute difference between the sums of the arrays is abs((3 + 9) - (7 + 3)) = 2.
```

Example 2:

```text
Input: nums = [-36,36]
Output: 72
Explanation: One optimal partition is: [-36] and [36].
The absolute difference between the sums of the arrays is abs((-36) - (36)) = 72.
```

Example 3:

![2035-2](https://assets.leetcode.com/uploads/2021/10/02/ex3.png)

```text
Input: nums = [2,-1,0,4,-2,-9]
Output: 0
Explanation: One optimal partition is: [2,4,-9] and [-1,0,-2].
The absolute difference between the sums of the arrays is abs((2 + 4 + -9) - (-1 + 0 + -2)) = 0.
```

__Constraints:__

- `1 <= n <= 15`
- `nums.length == 2 * n`
- `-10 ^ 7 <= nums[i] <= 10 ^ 7`

__题目描述:__

给你一个长度为 `2 * n` 的整数数组。你需要将 `nums` 分成 __两个__ 长度为 `n` 的数组，分别求出两个数组的和，并 __最小化__ 两个数组和之 _差的绝对值_ 。 `nums` 中每个元素都需要放入两个数组之一。

请你返回 最小 的数组和之差。

__示例:__

示例 1：

![2035-3](https://assets.leetcode.com/uploads/2021/10/02/ex1.png)

```text
输入：nums = [3,9,7,3]
输出：2
解释：最优分组方案是分成 [3,9] 和 [7,3] 。
数组和之差的绝对值为 abs((3 + 9) - (7 + 3)) = 2 。
```

示例 2：

```text
输入：nums = [-36,36]
输出：72
解释：最优分组方案是分成 [-36] 和 [36] 。
数组和之差的绝对值为 abs((-36) - (36)) = 72 。
```

示例 3：

![2035-4](https://assets.leetcode.com/uploads/2021/10/02/ex3.png)

```text
输入：nums = [2,-1,0,4,-2,-9]
输出：0
解释：最优分组方案是分成 [2,4,-9] 和 [-1,0,-2] 。
数组和之差的绝对值为 abs((2 + 4 + -9) - (-1 + 0 + -2)) = 0 。
```

__提示：__

- `1 <= n <= 15`
- `nums.length == 2 * n`
- `-10 ^ 7 <= nums[i] <= 10 ^ 7`

__思路:__

```text
状态压缩 ➕ 折半枚举
注意到 2 * n <= 30, 可以使用状态压缩的方法进行枚举, 但是直接枚举所有的状态复杂度为 O(2 ^ 30), 会超时
因此可以使用折半枚举的方法, 将数组分成两部分, 分别枚举两部分的状态, 时间复杂度为 O(2 ^ 15)
分别求出两个数组的子集的和记录在有序列表中
找到最接近数组和的一半的数组和, 然后在另一个数组中找到最接近的数组和, 更新结果
实际上是从 left 中取出 k 个数, 从 right 中取出 m - k 个数, 使得 sum(left) + sum(right) 最接近 sum / 2
时间复杂度为 O(N * 2 ^ N), 空间复杂度为 O(N * 2 ^ N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumDifference(vector<int>& nums) 
    {
        n = nums.size(), m = n >> 1, result = INT_MAX;
        q.resize(m + 1);
        dfs1(nums, 0, 0, 0);
        for (int i = 0; i <= m; i++) sort(q[i].begin(), q[i].end());
        dfs2(nums, m, 0, 0);
        return result;
    }
private:
    vector<vector<int>> q;
    int n, m, result;

    void dfs1(vector<int>& a, int u, int s, int c) 
    {
        if (u == m) 
        {
            q[c].push_back(s);
            return;
        }
        dfs1(a, u + 1, s + a[u], c + 1);
        dfs1(a, u + 1, s - a[u], c);
    }

    void dfs2(vector<int>& a, int u, int s, int c) 
    {
        if (c > m) return;
        if (u == n) 
        {
            auto& cur = q[c];
            int t = cur.size(), l = 0, r = t - 1;
            while (l < r) 
            {
                int mid = (l + r + 1) >> 1;
                if (cur[mid] <= s) l = mid;
                else r = mid - 1; 
            }
            result = min(result, abs(s - cur[l]));
            if (l + 1 < t) result = min(result, abs(s - cur[l + 1]));
            return;
        }
        dfs2(a, u + 1, s + a[u], c + 1);
        dfs2(a, u + 1, s - a[u], c);
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumDifference(int[] nums) {
        int result = Integer.MAX_VALUE, n = nums.length, m = n >> 1, total = 1 << m, sum = Arrays.stream(nums).sum(), left[] = new int[m], right[] = new int[m];
        TreeMap<Integer, TreeSet<Integer>> a = new TreeMap<>(), b = new TreeMap<>();
        for (int i = 0; i < m; i++) left[i] = nums[i];
        for (int i = m; i < n; i++) right[i - m] = nums[i];
        for (int mask = 0; mask < total; mask++) {
            int x = 0, y = 0, bit = Integer.bitCount(mask);
            for (int i = 0; i < m; i++) {
                if (((mask >> i) & 1) == 1) {
                    x += left[i];
                    y += right[i];
                }
            }
            TreeSet<Integer> p = a.getOrDefault(bit, new TreeSet<>()), q = b.getOrDefault(bit, new TreeSet<>());
            p.add(x);
            q.add(y);
            a.put(bit, p);
            b.put(bit, q);
        }
        for (Map.Entry<Integer, TreeSet<Integer>> entry : a.entrySet()) for (int i : entry.getValue()) if (b.get(m - entry.getKey()).floor((sum >> 1) - i) != null) result = Math.min(result, Math.abs((sum - i - b.get(m - entry.getKey()).floor((sum >> 1) - i)) - (i + b.get(m - entry.getKey()).floor((sum >> 1) - i))));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumDifference(self, nums: List[int]) -> int:
        return min(f([sum(left) for left in combinations(nums[:n], l)], sorted([sum(right) for right in combinations(nums[n:], n - l)]) + [inf], sum(nums)) for l in range(n + 1)) if (f := lambda a, b, ts: min(abs(ts - ls * 2 - b[bisect_left(b, (ts - ls * 2) >> 1)] * 2) for ls in a)) and (n := len(nums) >> 1) else inf
```
