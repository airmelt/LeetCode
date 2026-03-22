# 2570 Merge Two 2D Arrays by Summing Values 合并两个二维数组 求和法

__Description:__

You are given two __2D__ integer arrays `nums1` and `nums2.`

- `nums1[i] = [id_i, val_i]` indicate that the number with the id `id_i` has a value equal to `val_i`.
- `nums2[i] = [id_i, val_i]` indicate that the number with the id `id_i` has a value equal to `val_i`.

Each array contains unique ids and is sorted in ascending order by id.

Merge the two arrays into one array that is sorted in ascending order by id, respecting the following conditions:

- Only ids that appear in at least one of the two arrays should be included in the resulting array.
- Each id should be included __only once__ and its value should be the sum of the values of this id in the two arrays. If the id does not exist in one of the two arrays, then assume its value in that array to be `0`.

Return the resulting array. The returned array must be sorted in ascending order by id.

__Example:__

Example 1:

```text
Input: nums1 = [[1,2],[2,3],[4,5]], nums2 = [[1,4],[3,2],[4,1]]
Output: [[1,6],[2,3],[3,2],[4,6]]
Explanation: The resulting array contains the following:
```

- id = 1, the value of this id is 2 + 4 = 6.
- id = 2, the value of this id is 3.
- id = 3, the value of this id is 2.
- id = 4, the value of this id is 5 + 1 = 6.

Example 2:

```text
Input: nums1 = [[2,4],[3,6],[5,5]], nums2 = [[1,3],[4,3]]
Output: [[1,3],[2,4],[3,6],[4,3],[5,5]]
Explanation: There are no common ids, so we just include each id with its value in the resulting list.
```

__Constraints:__

- `1 <= nums1.length, nums2.length <= 200`
- `nums1[i].length == nums2[j].length == 2`
- `1 <= id_i, val_i <= 1000`
- Both arrays contain unique ids.
- Both arrays are in strictly ascending order by id.

__题目描述:__

给你两个 __二维__ 整数数组 `nums1` 和 `nums2.`

- `nums1[i] = [id_i, val_i]` 表示编号为 `id_i` 的数字对应的值等于 `val_i` 。
- `nums2[i] = [id_i, val_i]` 表示编号为 `id_i` 的数字对应的值等于 `val_i` 。

每个数组都包含 互不相同 的 id ，并按 id 以 递增 顺序排列。

请你将两个数组合并为一个按 id 以递增顺序排列的数组，并符合下述条件：

- 只有在两个数组中至少出现过一次的 id 才能包含在结果数组内。
- 每个 id 在结果数组中 __只能出现一次__ ，并且其对应的值等于两个数组中该 id 所对应的值求和。如果某个数组中不存在该 id ，则假定其对应的值等于 `0` 。

返回结果数组。返回的数组需要按 id 以递增顺序排列。

__示例:__

示例 1：

```text
输入：nums1 = [[1,2],[2,3],[4,5]], nums2 = [[1,4],[3,2],[4,1]]
输出：[[1,6],[2,3],[3,2],[4,6]]
解释：结果数组中包含以下元素：
```

- id = 1 ，对应的值等于 2 + 4 = 6 。
- id = 2 ，对应的值等于 3 。
- id = 3 ，对应的值等于 2 。
- id = 4 ，对应的值等于5 + 1 = 6 。

示例 2：

```text
输入：nums1 = [[2,4],[3,6],[5,5]], nums2 = [[1,3],[4,3]]
输出：[[1,3],[2,4],[3,6],[4,3],[5,5]]
解释：不存在共同 id ，在结果数组中只需要包含每个 id 和其对应的值。
```

__提示：__

- `1 <= nums1.length, nums2.length <= 200`
- `nums1[i].length == nums2[j].length == 2`
- `1 <= id_i, val_i <= 1000`
- 数组中的 id 互不相同
- 数据均按 id 以严格递增顺序排列

__思路:__

```text
模拟
类似归并排序
使用哈希表或者长度超过 1000 的数组记录每个 id 的值
累加 nums1 和 nums2 中相同 id 的值
将哈希表/数组转化为结果数组
时间复杂度为 O(M + N), 空间复杂度为 O(M + N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> mergeArrays(vector<vector<int>>& nums1, vector<vector<int>>& nums2) 
    {
        vector<int> nums(1001);
        for (const auto& a : nums1) nums[a.front()] += a.back();
        for (const auto& a : nums2) nums[a.front()] += a.back();
        vector<vector<int>> result;
        for (int i = 0; i < 1001; i++) if (nums[i]) result.emplace_back(vector<int>{i, nums[i]});
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] mergeArrays(int[][] nums1, int[][] nums2) {
        int[] nums = new int[1001];
        for (int[] a : nums1) nums[a[0]] += a[1];
        for (int[] a : nums2) nums[a[0]] += a[1];
        var result = new ArrayList<int[]>();
        for (int i = 0; i < 1001; i++) if (nums[i] > 0) result.add(new int[]{i, nums[i]});
        return result.toArray(new int[result.size()][]);
    }
}
```

__Python__:

```Python
class Solution:
    def mergeArrays(self, nums1: List[List[int]], nums2: List[List[int]]) -> List[List[int]]:
        return sorted((Counter(dict(nums1)) + Counter(dict(nums2))).items())
```
