# 2610 Convert an Array Into a 2D Array With Conditions 转换二维数组

__Description:__

You are given an integer array `nums`. You need to create a 2D array from `nums` satisfying the following conditions:

- The 2D array should contain __only__ the elements of the array `nums`.
- Each row in the 2D array contains __distinct__ integers.
- The number of rows in the 2D array should be __minimal__.

Return the resulting array. If there are multiple answers, return any of them.

Note that the 2D array can have a different number of elements on each row.

__Example:__

Example 1:

```text
Input: nums = [1,3,4,1,2,3,1]
Output: [[1,3,4,2],[1,3],[1]]
Explanation: We can create a 2D array that contains the following rows:
- 1,3,4,2
- 1,3
- 1
All elements of nums were used, and each row of the 2D array contains distinct integers, so it is a valid answer.
It can be shown that we cannot have less than 3 rows in a valid array.
```

Example 2:

```text
Input: nums = [1,2,3,4]
Output: [[4,3,2,1]]
Explanation: All elements of the array are distinct, so we can keep all of them in the first row of the 2D array.
```

__Constraints:__

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= nums.length`

__题目描述:__

给你一个整数数组 `nums` 。请你创建一个满足以下条件的二维数组:

- 二维数组应该 __只__ 包含数组 `nums` 中的元素。
- 二维数组中的每一行都包含 __不同__ 的整数。
- 二维数组的行数应尽可能 __少__ 。

返回结果数组。如果存在多种答案，则返回其中任何一种。

请注意，二维数组的每一行上可以存在不同数量的元素。

__示例:__

示例 1：

```text
输入：nums = [1,3,4,1,2,3,1]
输出：[[1,3,4,2],[1,3],[1]]
解释：根据题目要求可以创建包含以下几行元素的二维数组：
- 1,3,4,2
- 1,3
- 1
nums 中的所有元素都有用到，并且每一行都由不同的整数组成，所以这是一个符合题目要求的答案。
可以证明无法创建少于三行且符合题目要求的二维数组。
```

示例 2：

```text
输入：nums = [1,2,3,4]
输出：[[4,3,2,1]]
解释：nums 中的所有元素都不同，所以我们可以将其全部保存在二维数组中的第一行。
```

__提示：__

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= nums.length`

__思路:__

```text
哈希表
使用哈希表统计每个数出现次数
最终的结果行数为哈希表中最多出现次数
每次将当前哈希表的所有元素加入到结果中
并将哈希表中每个元素的出现次数减一
如果出现次数为 0, 则从哈希表中删除该元素
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> findMatrix(vector<int>& nums) 
    {
        vector<vector<int>> result;
        vector<int> m(nums.size() + 1);
        for (const auto& num : nums) 
        {
            int& c = m[num];
            if (c == result.size()) result.emplace_back();
            result[c++].emplace_back(num);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> findMatrix(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : nums) map.merge(num, 1, Integer::sum);
        List<List<Integer>> result = new ArrayList<>();
        while (!map.isEmpty()) {
            List<Integer> row = new ArrayList<>(map.keySet());
            result.add(row);
            for (int num : row) if (map.merge(num, -1, Integer::sum) == 0) map.remove(num);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findMatrix(self, nums: List[int]) -> List[List[int]]:
        m = (c := Counter(nums)).most_common()[0][1]
        result = [set() for _ in range(m)]
        for i in nums:
            for s in result:
                if i not in s:
                    s.add(i)
                    break
        return [list(s) for s in result]
```
