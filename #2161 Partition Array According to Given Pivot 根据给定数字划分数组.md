# 2161 Partition Array According to Given Pivot 根据给定数字划分数组

__Description:__

You are given a __0-indexed__ integer array `nums` and an integer `pivot`. Rearrange `nums` such that the following conditions are satisfied:

- Every element less than `pivot` appears __before__ every element greater than `pivot`.
- Every element equal to `pivot` appears __in between__ the elements less than and greater than `pivot`.
- The __relative order__ of the elements less than `pivot` and the elements greater than `pivot` is maintained.
  - More formally, consider every `pi`, `pj` where `pi` is the new position of the `i ^ th` element and `pj` is the new position of the `j ^ th` element. For elements less than `pivot`, if `i < j` and `nums[i] < pivot` and `nums[j] < pivot`, then `pi < pj`. Similarly for elements greater than `pivot`, if `i < j` and `nums[i] > pivot` and `nums[j] > pivot`, then `pi < pj`.

Return `nums` _after the rearrangement._

__Example:__

Example 1:

```text
Input: nums = [9,12,5,10,14,3,10], pivot = 10
Output: [9,5,3,10,10,12,14]
Explanation: 
The elements 9, 5, and 3 are less than the pivot so they are on the left side of the array.
The elements 12 and 14 are greater than the pivot so they are on the right side of the array.
The relative ordering of the elements less than and greater than pivot is also maintained. [9, 5, 3] and [12, 14] are the respective orderings.
```

Example 2:

```text
Input: nums = [-3,4,3,2], pivot = 2
Output: [-3,2,4,3]
Explanation: 
The element -3 is less than the pivot so it is on the left side of the array.
The elements 4 and 3 are greater than the pivot so they are on the right side of the array.
The relative ordering of the elements less than and greater than pivot is also maintained. [-3] and [4, 3] are the respective orderings.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 6 <= nums[i] <= 10 ^ 6`
- `pivot` equals to an element of `nums`.

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 和一个整数 `pivot` 。请你将 `nums` 重新排列，使得以下条件均成立:

- 所有小于 `pivot` 的元素都出现在所有大于 `pivot` 的元素 __之前__ 。
- 所有等于 `pivot` 的元素都出现在小于和大于 `pivot` 的元素 __中间__ 。
- 小于 `pivot` 的元素之间和大于 `pivot` 的元素之间的 __相对顺序__ 不发生改变。
  - 更正式的，考虑每一对 `pi`， `pj` ， `pi` 是初始时位置 `i` 元素的新位置， `pj` 是初始时位置 `j` 元素的新位置。对于小于 `pivot` 的元素，如果 `i < j` 且 `nums[i] < pivot` 和 `nums[j] < pivot` 都成立，那么 `pi < pj` 也成立。类似的，对于大于 `pivot` 的元素，如果 `i < j` 且 `nums[i] > pivot` 和 `nums[j] > pivot` 都成立，那么 `pi < pj` 。

请你返回重新排列 `nums` 数组后的结果数组。

__示例:__

示例 1：

```text
输入：nums = [9,12,5,10,14,3,10], pivot = 10
输出：[9,5,3,10,10,12,14]
解释：
元素 9 ，5 和 3 小于 pivot ，所以它们在数组的最左边。
元素 12 和 14 大于 pivot ，所以它们在数组的最右边。
小于 pivot 的元素的相对位置和大于 pivot 的元素的相对位置分别为 [9, 5, 3] 和 [12, 14] ，它们在结果数组中的相对顺序需要保留。
```

示例 2：

```text
输入：nums = [-3,4,3,2], pivot = 2
输出：[-3,2,4,3]
解释：
元素 -3 小于 pivot ，所以在数组的最左边。
元素 4 和 3 大于 pivot ，所以它们在数组的最右边。
小于 pivot 的元素的相对位置和大于 pivot 的元素的相对位置分别为 [-3] 和 [4, 3] ，它们在结果数组中的相对顺序需要保留。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 6 <= nums[i] <= 10 ^ 6`
- `pivot` 等于 `nums` 中的一个元素。

__思路:__

```text
模拟
将所有小于 pivot 的元素放在一个新数组 small 中
将所有大于 pivot 的元素放在一个新数组 big 中
先将 small 中的元素放回 nums 中
再将 pivot 放回 nums 中
最后将 big 中的元素放回 nums 中
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> pivotArray(vector<int>& nums, int pivot) 
    {
        vector<int> small, big;
        for (const auto& num : nums) 
        {
            if (num < pivot) small.emplace_back(num);
            if (num > pivot) big.emplace_back(num);
        }
        for (int i = 0, a = small.size(); i < a; i++) nums[i] = small[i];
        for (int n = nums.size(), a = small.size(), b = big.size(), i = a; i < n - b; i++) nums[i] = pivot;
        for (int n = nums.size(), a = small.size(), b = big.size(), i = n - b; i < n; i++) nums[i] = big[i + b - n];
        return nums; 
    }
};
```

__Java__:

```Java
class Solution {
    public int[] pivotArray(int[] nums, int pivot) {
        List<Integer> small = new ArrayList<>(), big = new ArrayList<>();
        for (int num : nums) {
            if (num < pivot) small.add(num);
            if (num > pivot) big.add(num);
        }
        for (int i = 0, a = small.size(); i < a; i++) nums[i] = small.get(i);
        for (int n = nums.length, a = small.size(), b = big.size(), i = a; i < n - b; i++) nums[i] = pivot;
        for (int n = nums.length, a = small.size(), b = big.size(), i = n - b; i < n; i++) nums[i] = big.get(i + b - n);
        return nums;
    }
}
```

__Python__:

```Python
class Solution:
    def pivotArray(self, nums: List[int], pivot: int) -> List[int]:
        return [num for num in nums if num < pivot] + [pivot] * nums.count(pivot) + [num for num in nums if num > pivot]
```
