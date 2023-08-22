# 1764 Form Array by Concatenating Subarrays of Another Array 通过连接另一个数组的子数组得到一个数组

__Description:__

You are given a 2D integer array `groups` of length `n`. You are also given an integer array `nums`.

You are asked if you can choose `n` __disjoint__ subarrays from the array `nums` such that the `i ^ th` subarray is equal to `groups[i]` ( _0-indexed_ ), and if `i > 0`, the `(i-1) ^ th` subarray appears __before__ the `i ^ th` subarray in `nums` (i.e. the subarrays must be in the same order as `groups`).

Return `true` _if you can do this task, and_ `false` _otherwise_.

Note that the subarrays are __disjoint__ if and only if there is no index `k` such that `nums[k]` belongs to more than one subarray. A subarray is a contiguous sequence of elements within an array.

__Example:__

Example 1:

```text
Input: groups = [[1,-1,-1],[3,-2,0]], nums = [1,-1,0,1,-1,-1,3,-2,0]
Output: true
Explanation: You can choose the 0th subarray as [1,-1,0,1,-1,-1,3,-2,0] and the 1st one as [1,-1,0,1,-1,-1,3,-2,0].
These subarrays are disjoint as they share no common nums[k] element.
```

Example 2:

```text
Input: groups = [[10,-2],[1,2,3,4]], nums = [1,2,3,4,10,-2]
Output: false
Explanation: Note that choosing the subarrays [1,2,3,4,10,-2] and [1,2,3,4,10,-2] is incorrect because they are not in the same order as in groups.
[10,-2] must come before [1,2,3,4].
```

Example 3:

```text
Input: groups = [[1,2,3],[3,4]], nums = [7,7,1,2,3,4,7,7]
Output: false
Explanation: Note that choosing the subarrays [7,7,1,2,3,4,7,7] and [7,7,1,2,3,4,7,7] is invalid because they are not disjoint.
They share a common elements nums[4] (0-indexed).
```

__Constraints:__

- `groups.length == n`
- `1 <= n <= 10 ^ 3`
- `1 <= groups[i].length, sum(groups[i].length) <= 10 ^ <span style="font-size: 10.8333px;">3</span>`
- `1 <= nums.length <= 10 ^ 3`
- `-10 ^ 7 <= groups[i][j], nums[k] <= 10 ^ 7`

__题目描述:__

给你一个长度为 `n` 的二维整数数组 `groups` ，同时给你一个整数数组 `nums` 。

你是否可以从 `nums` 中选出 `n` 个 __不相交__ 的子数组，使得第 `i` 个子数组与 `groups[i]` （下标从 __0__ 开始）完全相同，且如果 `i > 0` ，那么第 `(i-1)` 个子数组在 `nums` 中出现的位置在第 `i` 个子数组前面。（也就是说，这些子数组在 `nums` 中出现的顺序需要与 `groups` 顺序相同）

如果你可以找出这样的 `n` 个子数组，请你返回 `true` ，否则返回 `false` 。

如果不存在下标为 `k` 的元素 `nums[k]` 属于不止一个子数组，就称这些子数组是 __不相交__ 的。子数组指的是原数组中连续元素组成的一个序列。

__示例:__

示例 1：

```text
输入：groups = [[1,-1,-1],[3,-2,0]], nums = [1,-1,0,1,-1,-1,3,-2,0]
输出：true
解释：你可以分别在 nums 中选出第 0 个子数组 [1,-1,0,1,-1,-1,3,-2,0] 和第 1 个子数组 [1,-1,0,1,-1,-1,3,-2,0] 。
这两个子数组是不相交的，因为它们没有任何共同的元素。
```

示例 2：

```text
输入：groups = [[10,-2],[1,2,3,4]], nums = [1,2,3,4,10,-2]
输出：false
解释：选择子数组 [1,2,3,4,10,-2] 和 [1,2,3,4,10,-2] 是不正确的，因为它们出现的顺序与 groups 中顺序不同。
[10,-2] 必须出现在 [1,2,3,4] 之前。
```

示例 3：

```text
输入：groups = [[1,2,3],[3,4]], nums = [7,7,1,2,3,4,7,7]
输出：false
解释：选择子数组 [7,7,1,2,3,4,7,7] 和 [7,7,1,2,3,4,7,7] 是不正确的，因为它们不是不相交子数组。
它们有一个共同的元素 nums[4] （下标从 0 开始）。
```

__提示：__

- `groups.length == n`
- `1 <= n <= 10 ^ 3`
- `1 <= groups[i].length, sum(groups[i].length) <= 10 ^ <span>3</span>`
- `1 <= nums.length <= 10 ^ 3`
- `-10 ^ 7 <= groups[i][j], nums[k] <= 10 ^ 7`

__思路:__

```text
1. 贪心 ➕ 双指针
遍历 nums 数组
如果 nums[j] == groups[i][0] 相等
检查 nums[j:j + groups[i].size()] 是否等于 groups[i]
否则 j 自增
最后检查是否遍历完 groups 数组
时间复杂度为 O(MN), 空间复杂度为 O(1)
2. KMP
类似于从 nums 数组中找到 groups 数组的子串
时间复杂度为 O(M + N), 空间复杂度为 O(M)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool canChoose(vector<vector<int>>& groups, vector<int>& nums) 
    {
        vector<int> next = get_next(groups.front());
        int pre = kmp(groups.front(), nums, next, 0), n = groups.size();
        if (pre == -1) return false;
        for (int i = 1; i < n; i++)
        {
            int cur = 0, start = pre + groups[i - 1].size();
            next = get_next(groups[i]);
            if ((cur = kmp(groups[i], nums, next, start)) == -1) return false;
            pre = cur;
        }    
        return true;
    }
private:
    vector<int> get_next(vector<int>& pattern)
    {
        vector<int> next(pattern.size());
        if (pattern.size() > 1)
        {
            for (int i = 2, j = 0; i < next.size(); i++)
            {
                while (j and pattern[i - 1] != pattern[j]) j = next[j];
                if (pattern[j] == pattern[i - 1]) ++j;
                next[i] = j;
            }
        }
        return next;
    }

    int kmp(vector<int>& pattern, vector<int>& nums, vector<int> next, int start)
    {
        for (int i = start, j = 0, n = nums.size(), m = pattern.size(); i < n; i++)
        {
            while (j and nums[i] != pattern[j]) j = next[j];
            if (nums[i] == pattern[j]) ++j;
            if (j == m) return i - j + 1;
        }
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canChoose(int[][] groups, int[] nums) {
        int i = 0, j = 0, m = groups.length, n = nums.length;
        while (i < m && j < n) {
            if (check(groups[i], nums, j)) j += groups[i++].length;
            else ++j;
        }
        return i == m;
    }

    private boolean check(int[] group, int[] nums, int j) {
        if (j + group.length > nums.length) return false;
        for (int k = 0; k < group.length; k++) if (group[k] != nums[k + j]) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def canChoose(self, groups: List[List[int]], nums: List[int]) -> bool:
        return not not re.search(r'(?<=[ [])' + r',( [ \-\d]+,)* '.join(str(ls)[1:-1] for ls in groups) + r'(?=[,\]])', str(nums))
```
