# 3028 Ant on the Boundary 边界上的蚂蚁

__Description:__

An ant is on a boundary. It sometimes goes left and sometimes right.

You are given an array of __non-zero__ integers `nums`. The ant starts reading `nums` from the first element of it to its end. At each step, it moves according to the value of the current element:

- If `nums[i] < 0`, it moves __left__ by `-nums[i]` units.
- If `nums[i] > 0`, it moves __right__ by `nums[i]` units.

Return the number of times the ant returns to the boundary.

Notes:

- There is an infinite space on both sides of the boundary.
- We check whether the ant is on the boundary only after it has moved `|nums[i]|` units. In other words, if the ant crosses the boundary during its movement, it does not count.

__Example:__

Example 1:

```text
Input: nums = [2,3,-5]
Output: 1
Explanation: After the first step, the ant is 2 steps to the right of the boundary.
After the second step, the ant is 5 steps to the right of the boundary.
After the third step, the ant is on the boundary.
So the answer is 1.
```

Example 2:

```text
Input: nums = [3,2,-3,-4]
Output: 0
Explanation: After the first step, the ant is 3 steps to the right of the boundary.
After the second step, the ant is 5 steps to the right of the boundary.
After the third step, the ant is 2 steps to the right of the boundary.
After the fourth step, the ant is 2 steps to the left of the boundary.
The ant never returned to the boundary, so the answer is 0.
```

__Constraints:__

- `1 <= nums.length <= 100`
- `-10 <= nums[i] <= 10`
- `nums[i] != 0`

__题目描述:__

边界上有一只蚂蚁，它有时向 左 走，有时向 右 走。

给你一个 __非零__ 整数数组 `nums` 。蚂蚁会按顺序读取 `nums` 中的元素，从第一个元素开始直到结束。每一步，蚂蚁会根据当前元素的值移动:

- 如果 `nums[i] < 0` ，向 __左__ 移动 `-nums[i]`单位。
- 如果 `nums[i] > 0` ，向 __右__ 移动 `nums[i]`单位。

返回蚂蚁 返回 到边界上的次数。

注意：

- 边界两侧有无限的空间。
- 只有在蚂蚁移动了 `|nums[i]|` 单位后才检查它是否位于边界上。换句话说，如果蚂蚁只是在移动过程中穿过了边界，则不会计算在内。

__示例:__

示例 1：

```text
输入：nums = [2,3,-5]
输出：1
解释：第 1 步后，蚂蚁距边界右侧 2 单位远。
第 2 步后，蚂蚁距边界右侧 5 单位远。
第 3 步后，蚂蚁位于边界上。
所以答案是 1 。
```

示例 2：

```text
输入：nums = [3,2,-3,-4]
输出：0
解释：第 1 步后，蚂蚁距边界右侧 3 单位远。
第 2 步后，蚂蚁距边界右侧 5 单位远。
第 3 步后，蚂蚁距边界右侧 2 单位远。
第 4 步后，蚂蚁距边界左侧 2 单位远。
蚂蚁从未返回到边界上，所以答案是 0 。
```

__提示：__

- `1 <= nums.length <= 100`
- `-10 <= nums[i] <= 10`
- `nums[i] != 0`

__思路:__

```text
前缀和
统计前缀和中 0 出现的次数即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int returnToBoundaryCount(vector<int>& nums) 
    {
        return count_if(nums.begin(), nums.end(), [sum = 0](int x) mutable { return !(sum += x); });
    }
};
```

__Java__:

```Java
class Solution {
    public int returnToBoundaryCount(int[] nums) {
        return Arrays.stream(nums).collect(() -> new int[2], (a, x) -> a[1] += ((a[0] += x) == 0 ? 1 : 0), (a, b) -> {})[1];
    }
}
```

__Python__:

```Python
class Solution:
    def returnToBoundaryCount(self, nums: List[int]) -> int:
        return list(accumulate(nums)).count(0)
```
