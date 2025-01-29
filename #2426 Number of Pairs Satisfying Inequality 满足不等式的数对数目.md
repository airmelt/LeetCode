# 2426 Number of Pairs Satisfying Inequality 满足不等式的数对数目

__Description:__

You are given two __0-indexed__ integer arrays `nums1` and `nums2`, each of size `n`, and an integer `diff`. Find the number of __pairs__ `(i, j)` such that:

- `0 <= i < j <= n - 1` __and__
- `nums1[i] - nums1[j] <= nums2[i] - nums2[j] + diff`.

Return the number of pairs that satisfy the conditions.

__Example:__

Example 1:

```text
Input: nums1 = [3,2,5], nums2 = [2,2,1], diff = 1
Output: 3
Explanation:
There are 3 pairs that satisfy the conditions:
1. i = 0, j = 1: 3 - 2 <= 2 - 2 + 1. Since i < j and 1 <= 1, this pair satisfies the conditions.
2. i = 0, j = 2: 3 - 5 <= 2 - 1 + 1. Since i < j and -2 <= 2, this pair satisfies the conditions.
3. i = 1, j = 2: 2 - 5 <= 2 - 1 + 1. Since i < j and -3 <= 2, this pair satisfies the conditions.
Therefore, we return 3.
```

Example 2:

```text
Input: nums1 = [3,-1], nums2 = [-2,2], diff = -1
Output: 0
Explanation:
Since there does not exist any pair that satisfies the conditions, we return 0.
```

__Constraints:__

- `n == nums1.length == nums2.length`
- `2 <= n <= 10 ^ 5`
- `-10 ^ 4 <= nums1[i], nums2[i] <= 10 ^ 4`
- `-10 ^ 4 <= diff <= 10 ^ 4`

__题目描述:__

给你两个下标从 __0__ 开始的整数数组 `nums1` 和 `nums2` ，两个数组的大小都为 `n` ，同时给你一个整数 `diff` ，统计满足以下条件的 __数对__ `(i, j)` :

- `0 <= i < j <= n - 1` _且_
- `nums1[i] - nums1[j] <= nums2[i] - nums2[j] + diff`.

请你返回满足条件的 数对数目 。

__示例:__

示例 1：

```text
输入：nums1 = [3,2,5], nums2 = [2,2,1], diff = 1
输出：3
解释：
总共有 3 个满足条件的数对：
1. i = 0, j = 1：3 - 2 <= 2 - 2 + 1 。因为 i < j 且 1 <= 1 ，这个数对满足条件。
2. i = 0, j = 2：3 - 5 <= 2 - 1 + 1 。因为 i < j 且 -2 <= 2 ，这个数对满足条件。
3. i = 1, j = 2：2 - 5 <= 2 - 1 + 1 。因为 i < j 且 -3 <= 2 ，这个数对满足条件。
所以，我们返回 3 。
```

示例 2：

```text
输入：nums1 = [3,-1], nums2 = [-2,2], diff = -1
输出：0
解释：
没有满足条件的任何数对，所以我们返回 0 。
```

__提示：__

- `n == nums1.length == nums2.length`
- `2 <= n <= 10 ^ 5`
- `-10 ^ 4 <= nums1[i], nums2[i] <= 10 ^ 4`
- `-10 ^ 4 <= diff <= 10 ^ 4`

__思路:__

```text
数状数组
将式子 nums1[i] - nums1[j] <= nums2[i] - nums2[j] + diff 转化为 nums1[i] - nums2[i] <= nums1[j] - nums2[j] + diff
然后使用数状数组及二分查找检查每个元素的的位置
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class BIT 
{
public:
    BIT(int n) : tree(n) {}

    void add(int x) 
    {
        while (x < tree.size()) 
        {
            ++tree[x];
            x += x & -x;
        }
    }

    int query(int x) 
    {
        int result = 0;
        while (x > 0) 
        {
            result += tree[x];
            x &= x - 1;
        }
        return result;
    }
private:
    vector<int> tree;
};

class Solution 
{
public:
    long long numberOfPairs(vector<int>& nums1, vector<int>& nums2, int diff) {
        int n = nums1.size();
        for (int i = 0; i < n; i++) nums1[i] -= nums2[i];
        auto b = nums1;
        sort(b.begin(), b.end());
        b.erase(unique(b.begin(), b.end()), b.end());
        long result = 0L;
        auto t = new BIT(n + 1);
        for (const auto& x : nums1) 
        {
            result += t -> query(upper_bound(b.begin(), b.end(), x + diff) - b.begin());
            t -> add(lower_bound(b.begin(), b.end(), x) - b.begin() + 1);
        }
        return result;
    }
};
```

__Java__:

```Java
LeetCode Logo
题库
1150

avatar
Plus 会员
调试中...
调试中...







运行
题目描述
题解
题解
提交记录
提交记录
代码


测试用例
测试用例
测试结果
2426. 满足不等式的数对数目
已解答
困难
相关标签
相关企业
提示
给你两个下标从 0 开始的整数数组 nums1 和 nums2 ，两个数组的大小都为 n ，同时给你一个整数 diff ，统计满足以下条件的 数对 (i, j) ：

0 <= i < j <= n - 1 且
nums1[i] - nums1[j] <= nums2[i] - nums2[j] + diff.
请你返回满足条件的 数对数目 。

 

示例 1：

输入：nums1 = [3,2,5], nums2 = [2,2,1], diff = 1
输出：3
解释：
总共有 3 个满足条件的数对：
1. i = 0, j = 1：3 - 2 <= 2 - 2 + 1 。因为 i < j 且 1 <= 1 ，这个数对满足条件。
2. i = 0, j = 2：3 - 5 <= 2 - 1 + 1 。因为 i < j 且 -2 <= 2 ，这个数对满足条件。
3. i = 1, j = 2：2 - 5 <= 2 - 1 + 1 。因为 i < j 且 -3 <= 2 ，这个数对满足条件。
所以，我们返回 3 。
示例 2：

输入：nums1 = [3,-1], nums2 = [-2,2], diff = -1
输出：0
解释：
没有满足条件的任何数对，所以我们返回 0 。
 

提示：

n == nums1.length == nums2.length
2 <= n <= 105
-104 <= nums1[i], nums2[i] <= 104
-104 <= diff <= 104
面试中遇到过这道题?
1/5
是
否
通过次数
5.6K
提交次数
11.8K
通过率
47.5%
相关标签
相关企业
提示 1
提示 2
提示 3
相似题目
评论 (51)





展开全部


展开全部

展开全部



展开全部
展开全部

展开全部

展开全部
展开全部
贡献者

© 2025 领扣网络（上海）有限公司

24

51



1 人在线
Java
智能模式




192021222324252627282930
class Solution {
    public long numberOfPairs(int[] nums1, int[] nums2, int diff) {
        int n = nums1.length;
        for (int i = 0; i < n; i++) nums1[i] -= nums2[i];
        var b = Arrays.stream(nums1).distinct().sorted().toArray();
        long result = 0L;
        var t = new BIT(b.length + 1);
        for (int x : nums1) {
            result += t.query(lowerBound(b, x + diff + 1));
            t.add(lowerBound(b, x) + 1);

已从本地恢复
升级云端代码存储
通过
执行用时: 0 ms
Case 1
Case 2
输入
nums1 =
[3,2,5]
nums2 =
[2,2,1]
diff =
1
输出
3
预期结果
3
贡献测试用例




```

__Python__:

```Python
from sortedcontainers import SortedList
class Solution:
    def numberOfPairs(self, nums1: List[int], nums2: List[int], diff: int) -> int:
        result, sl = 0, SortedList()
        for x in (a - b for a, b in zip(nums1, nums2)):
            result += sl.bisect_right(x + diff)
            sl.add(x)
        return result
```
