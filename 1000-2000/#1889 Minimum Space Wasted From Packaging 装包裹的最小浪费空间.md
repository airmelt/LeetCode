# 1889 Minimum Space Wasted From Packaging 装包裹的最小浪费空间

__Description:__

You have `n` packages that you are trying to place in boxes, __one package in each box__. There are `m` suppliers that each produce boxes of __different sizes__ (with infinite supply). A package can be placed in a box if the size of the package is __less than or equal to__ the size of the box.

The package sizes are given as an integer array `packages`, where `packages[i]` is the __size__ of the `i ^ th` package. The suppliers are given as a 2D integer array `boxes`, where `boxes[j]` is an array of __box sizes__ that the `j ^ th` supplier produces.

You want to choose a __single supplier__ and use boxes from them such that the __total wasted space__ is __minimized__. For each package in a box, we define the space __wasted__ to be `size of the box - size of the package`. The __total wasted space__ is the sum of the space wasted in __all__ the boxes.

- For example, if you have to fit packages with sizes `[2,3,5]` and the supplier offers boxes of sizes `[4,8]`, you can fit the packages of size- `2` and size- `3` into two boxes of size- `4` and the package with size- `5` into a box of size- `8`. This would result in a waste of `(4-2) + (4-3) + (8-5) = 6`.

Return _the __minimum total wasted space__ by choosing the box supplier __optimally__, or_ `-1` _if it is __impossible__ to fit all the packages inside boxes._ Since the answer may be __large__, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: packages = [2,3,5], boxes = [[4,8],[2,8]]
Output: 6
Explanation: It is optimal to choose the first supplier, using two size-4 boxes and one size-8 box.
The total waste is (4-2) + (4-3) + (8-5) = 6.
```

Example 2:

```text
Input: packages = [2,3,5], boxes = [[1,4],[2,3],[3,4]]
Output: -1
Explanation: There is no box that the package of size 5 can fit in.
```

Example 3:

```text
Input: packages = [3,5,8,10,11,12], boxes = [[12],[11,9],[10,5,14]]
Output: 9
Explanation: It is optimal to choose the third supplier, using two size-5 boxes, two size-10 boxes, and two size-14 boxes.
The total waste is (5-3) + (5-5) + (10-8) + (10-10) + (14-11) + (14-12) = 9.
```

__Constraints:__

- `n == packages.length`
- `m == boxes.length`
- `1 <= n <= 10 ^ 5`
- `1 <= m <= 10 ^ 5`
- `1 <= packages[i] <= 10 ^ 5`
- `1 <= boxes[j].length <= 10 ^ 5`
- `1 <= boxes[j][k] <= 10 ^ 5`
- `sum(boxes[j].length) <= 10 ^ 5`
- The elements in `boxes[j]` are __distinct__.

__题目描述:__

给你 `n` 个包裹，你需要把它们装在箱子里，__每个箱子装一个包裹__。总共有 `m` 个供应商提供 __不同尺寸__ 的箱子（每个规格都有无数个箱子）。如果一个包裹的尺寸 __小于等于__ 一个箱子的尺寸，那么这个包裹就可以放入这个箱子之中。

包裹的尺寸用一个整数数组 `packages` 表示，其中 `packages[i]` 是第 `i` 个包裹的尺寸。供应商用二维数组 `boxes` 表示，其中 `boxes[j]` 是第 `j` 个供应商提供的所有箱子尺寸的数组。

你想要选择 __一个供应商__ 并只使用该供应商提供的箱子，使得 __总浪费空间最小__ 。对于每个装了包裹的箱子，我们定义 __浪费的__ 空间等于 `箱子的尺寸 - 包裹的尺寸` 。__总浪费空间__ 为 __所有__ 箱子中浪费空间的总和。

- 比方说，如果你想要用尺寸数组为 `[4,8]` 的箱子装下尺寸为 `[2,3,5]` 的包裹，你可以将尺寸为 `2` 和 `3` 的两个包裹装入两个尺寸为 `4` 的箱子中，同时把尺寸为 `5` 的包裹装入尺寸为 `8` 的箱子中。总浪费空间为 `(4-2) + (4-3) + (8-5) = 6` 。

请你选择 __最优__ 箱子供应商，使得 __总浪费空间最小__ 。如果 __无法__ 将所有包裹放入箱子中，请你返回 `-1` 。由于答案可能会 __很大__ ，请返回它对 `10 ^ 9 + 7` _取余_ 的结果。

__示例:__

示例 1：

```text
输入：packages = [2,3,5], boxes = [[4,8],[2,8]]
输出：6
解释：选择第一个供应商最优，用两个尺寸为 4 的箱子和一个尺寸为 8 的箱子。
总浪费空间为 (4-2) + (4-3) + (8-5) = 6 。
```

示例 2：

```text
输入：packages = [2,3,5], boxes = [[1,4],[2,3],[3,4]]
输出：-1
解释：没有箱子能装下尺寸为 5 的包裹。
```

示例 3：

```text
输入：packages = [3,5,8,10,11,12], boxes = [[12],[11,9],[10,5,14]]
输出：9
解释：选择第三个供应商最优，用两个尺寸为 5 的箱子，两个尺寸为 10 的箱子和两个尺寸为 14 的箱子。
总浪费空间为 (5-3) + (5-5) + (10-8) + (10-10) + (14-11) + (14-12) = 9 。
```

__提示：__

- `n == packages.length`
- `m == boxes.length`
- `1 <= n <= 10 ^ 5`
- `1 <= m <= 10 ^ 5`
- `1 <= packages[i] <= 10 ^ 5`
- `1 <= boxes[j].length <= 10 ^ 5`
- `1 <= boxes[j][k] <= 10 ^ 5`
- `sum(boxes[j].length) <= 10 ^ 5`
- `boxes[j]` 中的元素 __互不相同__ 。

__思路:__

```text
二分 ➕ 前缀和
对包裹进行排序, 并求出其前缀和 pre
对每个箱子进行排序
如果最大的箱子比最大的包裹要小, 肯定装不下, 不处理这个箱子
否则利用二分查找找到每个箱子对应的包裹的边界(即 bisect_right)
累加浪费的容量, 总容量为箱子的大小乘上包裹的数量, 包裹的重量可以用前缀和快速求出
时间复杂度为 O(NlogN + LlogL + LlogN), 空间复杂度为 O(N + logL), 其中 N 为 packages 数组的大小, L 为 boxes 数组中所有子数组的大小的和, 对包裹进行排序的时间复杂度为 O(NlogN), 对每个箱子进行排序的总时间复杂度为 O(LlogL), 对每个箱子都需要进行二分查找, 复杂度为 O(LlogN), 前缀和的时间复杂度为 O(N), 可以忽略不计, 空间复杂度为 O(N), 排序需要 O(logL) 和 O(logN), 后者可以忽略不计
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minWastedSpace(vector<int>& packages, vector<vector<int>>& boxes) 
    {
        sort(packages.begin(), packages.end());
        int n = packages.size(), pos = 0, MOD = 1e9 + 7;
        long long result = LLONG_MAX;
        vector<long long> pre(n + 1);
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + packages[i];
        for (auto& box : boxes) 
        {
            sort(box.begin(), box.end());
            if (packages.back() <= box.back()) 
            {
                long long cur = 0;
                int last = 0;
                for (const auto& b : box) 
                {
                    cur += (long long)b * ((pos = helper(packages, last, b)) - last) - (pre[pos] - pre[last]);
                    last = pos;
                }
                result = min(cur, result);
            }
        }
        return result == LLONG_MAX ? -1 : result % MOD;
    }
private: 
    int helper(vector<int>& packages, int last, int b) 
    {
        int left = last, right = packages.size();
        while (left < right) 
        {
            int mid = left + ((right - left) >> 1);
            if (packages[mid] > b) right = mid;
            else left = mid + 1;
        }
        return left;
    }
};
```

__Java__:

```Java
class Solution {
    public int minWastedSpace(int[] packages, int[][] boxes) {
        Arrays.sort(packages);
        int n = packages.length, pos = 0, MOD = 1_000_000_007;
        long result = Long.MAX_VALUE, pre[] = new long[n + 1];
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + packages[i];
        for (int[] box : boxes) {
            Arrays.sort(box);
            if (packages[packages.length - 1] <= box[box.length - 1]) {
                long cur = 0;
                int last = 0;
                for (int b : box) {
                    cur += (long)b * ((pos = helper(packages, last, b)) - last) - (pre[pos] - pre[last]);
                    last = pos;
                }
                result = Math.min(cur, result);
            }
        }
        return result == Long.MAX_VALUE ? -1 : (int)(result % MOD);
    }

    private int helper(int[] packages, int last, int b) {
        int left = last, right = packages.length;
        while (left < right) {
            int mid = left + ((right - left) >>> 1);
            if (packages[mid] > b) right = mid;
            else left = mid + 1;
        }
        return left;
    }
}
```

__Python__:

```Python
class Solution:
    def minWastedSpace(self, packages: List[int], boxes: List[List[int]]) -> int:
        packages.sort()
        n, pre, result, MOD = len(packages), [0] + list(accumulate(packages)), inf, 10 ** 9 + 7
        for box in boxes:
            box.sort()
            if packages[-1] <= box[-1]:
                cur, last = 0, 0
                for b in box:
                    pos = bisect.bisect_right(packages, b)
                    cur += b * (pos - last) - (pre[pos] - pre[last])
                    last = pos
                result = min(cur, result)
        return result % MOD if result < inf else -1
```
