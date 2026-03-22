# 2860 Happy Students 让所有学生保持开心的分组方法数

__Description:__

You are given a __0-indexed__ integer array `nums` of length `n` where `n` is the total number of students in the class. The class teacher tries to select a group of students so that all the students remain happy.

The `i ^ th` student will become happy if one of these two conditions is met:

- The student is selected and the total number of selected students is __strictly greater than__ `nums[i]`.
- The student is not selected and the total number of selected students is __strictly__ __less than__ `nums[i]`.

Return the number of ways to select a group of students so that everyone remains happy.

__Example:__

Example 1:

```text
Input: nums = [1,1]
Output: 2
Explanation: 
The two possible ways are:
The class teacher selects no student.
The class teacher selects both students to form the group. 
If the class teacher selects just one student to form a group then the both students will not be happy. Therefore, there are only two possible ways.
```

Example 2:

```text
Input: nums = [6,0,3,3,6,7,2,7]
Output: 3
Explanation: 
The three possible ways are:
The class teacher selects the student with index = 1 to form the group.
The class teacher selects the students with index = 1, 2, 3, 6 to form the group.
The class teacher selects all the students to form the group.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] < nums.length`

__题目描述:__

给你一个下标从 __0__ 开始、长度为 `n` 的整数数组 `nums` ，其中 `n` 是班级中学生的总数。班主任希望能够在让所有学生保持开心的情况下选出一组学生:

如果能够满足下述两个条件之一，则认为第 `i` 位学生将会保持开心:

- 这位学生被选中，并且被选中的学生人数 __严格大于__ `nums[i]` 。
- 这位学生没有被选中，并且被选中的学生人数 __严格小于__ `nums[i]` 。

返回能够满足让所有学生保持开心的分组方法的数目。

__示例:__

示例 1：

```text
输入：nums = [1,1]
输出：2
解释：
有两种可行的方法：
班主任没有选中学生。
班主任选中所有学生形成一组。 
如果班主任仅选中一个学生来完成分组，那么两个学生都无法保持开心。因此，仅存在两种可行的方法。
```

示例 2：

```text
输入：nums = [6,0,3,3,6,7,2,7]
输出：3
解释：
存在三种可行的方法：
班主任选中下标为 1 的学生形成一组。
班主任选中下标为 1、2、3、6 的学生形成一组。
班主任选中所有学生形成一组。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] < nums.length`

__思路:__

```text
排序
注意到 nums[i] 的范围为 [0, len(nums))
所以对于选 n 名学生一定是合法的
对于选择 k 名学生
要么小于 k 的 nums[i] 全部要选
大于 k 的 nums[i] 全部不能选
所以对于第 i 个学生如果 nums[i - 1] < i < nums[i] 就可以选
对于第一个学生只需要大于 0 即可
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countWays(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        int result = 1, n = nums.size();
        for (int i = 1; i < n; i++) result += nums[i - 1] < i and i < nums[i];
        return result + (nums.front() > 0);
    }
};
```

__Java__:

```Java
class Solution {
    public int countWays(List<Integer> nums) {
        Collections.sort(nums);
        int result = 1, n = nums.size();
        for (int i = 1; i < n; i++) if (nums.get(i - 1) < i && i < nums.get(i)) ++result;
        return result + (nums.get(0) > 0 ? 1 : 0);
    }
}
```

__Python__:

```Python
class Solution:
    def countWays(self, nums: List[int]) -> int:
        return sum(i > nums[i - 1] and nums[i] > i for i in range(1, len(nums))) + (nums[0] > 0) + 1 if (nums := sorted(nums)) else 0
```
