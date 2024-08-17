# 2215 Find the Difference of Two Arrays 找出两数组的不同

__Description:__

Given two __0-indexed__ integer arrays `nums1` and `nums2`, return _a list_ `answer` _of size_ `2` _where:_

- `answer[0]` _is a list of all __distinct__ integers in_ `nums1` _which are __not__ present in_ `nums2`_._
- `answer[1]` _is a list of all __distinct__ integers in_ `nums2` _which are __not__ present in_ `nums1`.

Note that the integers in the lists may be returned in any order.

__Example:__

Example 1:

```text
Input: nums1 = [1,2,3], nums2 = [2,4,6]
Output: [[1,3],[4,6]]
Explanation:
For nums1, nums1[1] = 2 is present at index 0 of nums2, whereas nums1[0] = 1 and nums1[2] = 3 are not present in nums2. Therefore, answer[0] = [1,3].
For nums2, nums2[0] = 2 is present at index 1 of nums1, whereas nums2[1] = 4 and nums2[2] = 6 are not present in nums2. Therefore, answer[1] = [4,6].
```

Example 2:

```text
Input: nums1 = [1,2,3,3], nums2 = [1,1,2,2]
Output: [[3],[]]
Explanation:
For nums1, nums1[2] and nums1[3] are not present in nums2. Since nums1[2] == nums1[3], their value is only included once and answer[0] = [3].
Every integer in nums2 is present in nums1. Therefore, answer[1] = [].
```

__Constraints:__

- `1 <= nums1.length, nums2.length <= 1000`
- `-1000 <= nums1[i], nums2[i] <= 1000`

__题目描述:__

给你两个下标从 `0` 开始的整数数组 `nums1` 和 `nums2` ，请你返回一个长度为 `2` 的列表 `answer` ，其中:

- `answer[0]` 是 `nums1` 中所有 __不__ 存在于 `nums2` 中的 __不同__ 整数组成的列表。
- `answer[1]` 是 `nums2` 中所有 __不__ 存在于 `nums1` 中的 __不同__ 整数组成的列表。

注意：列表中的整数可以按 任意 顺序返回。

__示例:__

示例 1：

```text
输入：nums1 = [1,2,3], nums2 = [2,4,6]
输出：[[1,3],[4,6]]
解释：
对于 nums1 ，nums1[1] = 2 出现在 nums2 中下标 0 处，然而 nums1[0] = 1 和 nums1[2] = 3 没有出现在 nums2 中。因此，answer[0] = [1,3]。
对于 nums2 ，nums2[0] = 2 出现在 nums1 中下标 1 处，然而 nums2[1] = 4 和 nums2[2] = 6 没有出现在 nums2 中。因此，answer[1] = [4,6]。
```

示例 2：

```text
输入：nums1 = [1,2,3,3], nums2 = [1,1,2,2]
输出：[[3],[]]
解释：
对于 nums1 ，nums1[2] 和 nums1[3] 没有出现在 nums2 中。由于 nums1[2] == nums1[3] ，二者的值只需要在 answer[0] 中出现一次，故 answer[0] = [3]。
nums2 中的每个整数都在 nums1 中出现，因此，answer[1] = [] 。
```

__提示：__

- `1 <= nums1.length, nums2.length <= 1000`
- `-1000 <= nums1[i], nums2[i] <= 1000`

__思路:__

```text
哈希表
使用哈希表记录 nums1 和 nums2 出现的元素
遍历 nums1 和 nums2, 将不同的元素加入到结果中
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> findDifference(vector<int>& nums1, vector<int>& nums2) 
    {
        set<int> set1(nums1.begin(), nums1.end()), set2(nums2.begin(), nums2.end());
        vector<vector<int>> result(2);
        set_difference(set1.begin(), set1.end(), set2.begin(), set2.end(), inserter(result.front(), begin(result.front())));
        set_difference(set2.begin(), set2.end(), set1.begin(), set1.end(), inserter(result.back(), begin(result.back())));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> findDifference(int[] nums1, int[] nums2) {
        return List.of(List.copyOf(Arrays.stream(nums1).boxed().collect(Collectors.toSet()).stream().filter(x -> !Arrays.stream(nums2).boxed().collect(Collectors.toSet()).contains(x)).collect(Collectors.toList())), List.copyOf(Arrays.stream(nums2).boxed().collect(Collectors.toSet())).stream().filter(x -> !Arrays.stream(nums1).boxed().collect(Collectors.toSet()).contains(x)).collect(Collectors.toList()));
    }
}
```

__Python__:

```Python
class Solution:
    def findDifference(self, nums1: List[int], nums2: List[int]) -> List[List[int]]:
        return [list(set(nums1) - set(nums2)), list(set(nums2) - set(nums1))]
```
