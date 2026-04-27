# 1630 Arithmetic Subarrays 等差子数组

__Description:__

A sequence of numbers is called __arithmetic__ if it consists of at least two elements, and the difference between every two consecutive elements is the same. More formally, a sequence `s` is arithmetic if and only if `s[i+1] - s[i] == s[1] - s[0]` for all valid `i`.

For example, these are arithmetic sequences:

1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9

The following sequence is not arithmetic:

1, 1, 2, 5, 7

You are given an array of `n` integers, `nums`, and two arrays of `m` integers each, `l` and `r`, representing the `m` range queries, where the `i ^ th` query is the range `[l[i], r[i]]`. All the arrays are __0-indexed__.

Return _a list of_ `boolean` _elements_ `answer`_, where_ `answer[i]` _is_ `true` _if the subarray_ `nums[l[i]], nums[l[i]+1], ... , nums[r[i]]` _can be __rearranged__ to form an __arithmetic__ sequence, and_ `false` _otherwise._

__Example:__

Example 1:

```text
__Input:__ nums = `[4,6,5,9,3,7]`, l = `[0,0,2]`, r = `[2,3,5]`
__Output:__ `[true,false,true]`
__Explanation:__
In the 0 ^ th query, the subarray is [4,6,5]. This can be rearranged as [6,5,4], which is an arithmetic sequence.
In the 1 ^ st query, the subarray is [4,6,5,9]. This cannot be rearranged as an arithmetic sequence.
In the 2 ^ nd query, the subarray is `[5,9,3,7]. This` can be rearranged as `[3,5,7,9]`, which is an arithmetic sequence.
```

Example 2:

```text
Input: nums = [-12,-9,-3,-12,-6,15,20,-25,-20,-15,-10], l = [0,1,6,4,8,7], r = [4,4,9,7,9,10]
Output: [false,true,false,false,true,true]
```

__Constraints:__

- `n == nums.length`
- `m == l.length`
- `m == r.length`
- `2 <= n <= 500`
- `1 <= m <= 500`
- `0 <= l[i] < r[i] < n`
- `-10 ^ 5 <= nums[i] <= 10 ^ 5`

__题目描述:__

如果一个数列由至少两个元素组成，且每两个连续元素之间的差值都相同，那么这个序列就是 __等差数列__ 。更正式地，数列 `s` 是等差数列，只需要满足:对于每个有效的 `i` ， `s[i+1] - s[i] == s[1] - s[0]` 都成立。

例如，下面这些都是 等差数列 ：

1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9

下面的数列 不是等差数列 ：

1, 1, 2, 5, 7

给你一个由 `n` 个整数组成的数组 `nums`，和两个由 `m` 个整数组成的数组 `l` 和 `r`，后两个数组表示 `m` 组范围查询，其中第 `i` 个查询对应范围 `[l[i], r[i]]` 。所有数组的下标都是 __从 0 开始__ 的。

返回 `boolean` 元素构成的答案列表 `answer` 。如果子数组 `nums[l[i]], nums[l[i]+1], ... , nums[r[i]]` 可以 __重新排列__ 形成 __等差数列__ ， `answer[i]` 的值就是 `true`；否则 `answer[i]` 的值就是 `false` 。

__示例:__

示例 1：

```text
__输入:__nums = `[4,6,5,9,3,7]`, l = `[0,0,2]`, r = `[2,3,5]`
__输出:__`[true,false,true]`
__解释:__
第 0 个查询，对应子数组 [4,6,5] 。可以重新排列为等差数列 [6,5,4] 。
第 1 个查询，对应子数组 [4,6,5,9] 。无法重新排列形成等差数列。
第 2 个查询，对应子数组 `[5,9,3,7] 。`可以重新排列为等差数列 `[3,5,7,9] 。`
```

示例 2：

```text
输入：nums = [-12,-9,-3,-12,-6,15,20,-25,-20,-15,-10], l = [0,1,6,4,8,7], r = [4,4,9,7,9,10]
输出：[false,true,false,false,true,true]
```

__提示：__

- `n == nums.length`
- `m == l.length`
- `m == r.length`
- `2 <= n <= 500`
- `1 <= m <= 500`
- `0 <= l[i] < r[i] < n`
- `-10 ^ 5 <= nums[i] <= 10 ^ 5`

__思路:__

```text
1. 排序
每次取出对应子数组, 排序检查是否为等差数列
时间复杂度为 O(MNlogN), 空间复杂度为 O(N)
2. 数学
对于每一次查询
找到子数组, 得到最小值和最大值
如果子数组只有单元素(最小值最大值相等), 可以为等差数列
如果公差不为整数, 不能为等差数列
否则检查最大值到最小值中间每个数是否都出现过
时间复杂度为 O(MN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<bool> checkArithmeticSubarrays(vector<int>& nums, vector<int>& l, vector<int>& r) 
    {
        vector<bool> result;
        for (int i = 0, m = l.size();i < m; i++)
        {
            vector<int> arr(nums.begin() + l[i], nums.begin() + r[i] + 1);
            sort(arr.begin(), arr.end());
            int d = arr[1] - arr[0], cur = 1;
            for (int j = 2; j <= r[i] - l[i]; j++)
            {
                if (d != arr[j] - arr[j - 1])
                {
                    cur = 0;
                    break;
                }
            }
            result.emplace_back(cur);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Boolean> checkArithmeticSubarrays(int[] nums, int[] l, int[] r) {
        List<Boolean> result = new ArrayList<>();
        for (int i = 0, m = l.length; i < m; i++) result.add(check(nums, l[i], r[i]));
        return result;
    }
    
    private boolean check(int nums[], int l, int r) {
        int max = nums[l], min = nums[l];
        for (int i = l + 1; i <= r; i++) {
            max = Math.max(max, nums[i]);
            min = Math.min(min, nums[i]);
        }
        int d = (max - min) / (r - l);
        if (max == min) return true;
        if (d * (r - l) != (max - min)) return false;
        boolean visited[] = new boolean[r - l + 1];
        for (int i = l, cur; i <= r; i++) {
            if ((nums[i] - min) % d != 0 || visited[cur = (nums[i] - min) / d]) return false;
            visited[cur] = true;
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def checkArithmeticSubarrays(self, nums: List[int], l: List[int], r: List[int]) -> List[bool]:
        return [len({x - y for x, y in zip(sorted(nums[l[i]:r[i] + 1])[1:], sorted(nums[l[i]:r[i] + 1])[:-1])}) == 1 for i in range(len(l))]
```
