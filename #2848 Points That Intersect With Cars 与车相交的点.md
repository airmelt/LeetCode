# 2848 Points That Intersect With Cars 与车相交的点

__Description:__

You are given a __0-indexed__ 2D integer array `nums` representing the coordinates of the cars parking on a number line. For any index `i`, `nums[i] = [start_i, end_i]` where `start_i` is the starting point of the `i ^ th` car and `end_i` is the ending point of the `i ^ th` car.

Return the number of integer points on the line that are covered with any part of a car.

__Example:__

Example 1:

```text
Input: nums = [[3,6],[1,5],[4,7]]
Output: 7
Explanation: All the points from 1 to 7 intersect at least one car, therefore the answer would be 7.
```

Example 2:

```text
Input: nums = [[1,3],[5,8]]
Output: 7
Explanation: Points intersecting at least one car are 1, 2, 3, 5, 6, 7, 8. There are a total of 7 points, therefore the answer would be 7.
```

__Constraints:__

- `1 <= nums.length <= 100`
- `nums[i].length == 2`
- `1 <= start_i <= end_i <= 100`

__题目描述:__

给你一个下标从 __0__ 开始的二维整数数组 `nums` 表示汽车停放在数轴上的坐标。对于任意下标 `i`， `nums[i] = [start_i, end_i]` ，其中 `start_i` 是第 `i` 辆车的起点， `end_i` 是第 `i` 辆车的终点。

返回数轴上被车 任意部分 覆盖的整数点的数目。

__示例:__

示例 1：

```text
输入：nums = [[3,6],[1,5],[4,7]]
输出：7
解释：从 1 到 7 的所有点都至少与一辆车相交，因此答案为 7 。
```

示例 2：

```text
输入：nums = [[1,3],[5,8]]
输出：7
解释：1、2、3、5、6、7、8 共计 7 个点满足至少与一辆车相交，因此答案为 7 。
```

__提示：__

- `1 <= nums.length <= 100`
- `nums[i].length == 2`
- `1 <= start_i <= end_i <= 100`

__思路:__

```text
1. 模拟 (Python)
用哈希集合直接存放所有元素
哈希集合自带去重
时间复杂度为 O(MN), 空间复杂度为 O(M)
2. 差分数组 (Java)
使用一个长度为 max(nums) + 2 的数组
使用差分数组记录下 nums 中出现过的元素
统计出现过的所有元素累加结果
时间复杂度为 O(M + N), 空间复杂度为 O(M)
3. 排序 (Cpp)
按照起点排序
将所有终点到起点的距离累加到结果
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
其中 M 为数组的值域, N 为数组的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfPoints(vector<vector<int>>& nums) 
    {
        sort(nums.begin(), nums.end(), [&](const auto x, const auto y) { return x.front() < y.front(); });
        int result = 0, start = 0;
        for (const auto& num : nums) 
        {
            if (start < num.back()) 
            {
                result += num.back() - max(start, num.front() - 1);
                start = num.back();
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfPoints(List<List<Integer>> nums) {
        int end = nums.stream().map(num -> num.get(1)).mapToInt(v -> v).max().getAsInt(), result = 0, s = 0, diff[] = new int[end + 2];
        for (var interval : nums) {
            ++diff[interval.get(0)];
            --diff[interval.get(1) + 1];
        }
        for (int v : diff) {
            s += v;
            if (s > 0) ++result;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfPoints(self, nums: List[List[int]]) -> int:
        return len(reduce(lambda x, y: x | y, [set(i for i in range(low, high + 1)) for low, high in nums]))
```
