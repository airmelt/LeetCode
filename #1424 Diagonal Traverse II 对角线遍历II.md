# 1424 Diagonal Traverse II 对角线遍历II

__Description:__

Given a 2D integer array `nums`, return _all elements of_ `nums` _in diagonal order as shown in the below images_.

__Example:__

Example 1:

![1424-1](https://assets.leetcode.com/uploads/2020/04/08/sample_1_1784.png)

```text
Input: nums = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,4,2,7,5,3,8,6,9]
```

Example 2:

![1424-2](https://assets.leetcode.com/uploads/2020/04/08/sample_2_1784.png)

```text
Input: nums = [[1,2,3,4,5],[6,7],[8],[9,10,11],[12,13,14,15,16]]
Output: [1,6,2,8,7,3,9,4,12,10,5,13,11,14,15,16]
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i].length <= 10 ^ 5`
- `1 <= sum(nums[i].length) <= 10 ^ 5`
- `1 <= nums[i][j] <= 10 ^ 5`

__题目描述:__

给你一个列表 `nums` ，里面每一个元素都是一个整数列表。请你依照下面各图的规则，按顺序返回 `nums` 中对角线上的整数。

__示例:__

示例 1：

![1424-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/04/23/sample_1_1784.png)

```text
输入：nums = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,4,2,7,5,3,8,6,9]
```

示例 2：

![1424-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/04/23/sample_2_1784.png)

```text
输入：nums = [[1,2,3,4,5],[6,7],[8],[9,10,11],[12,13,14,15,16]]
输出：[1,6,2,8,7,3,9,4,12,10,5,13,11,14,15,16]
```

示例 3：

```text
输入：nums = [[1,2,3],[4],[5,6,7],[8],[9,10,11]]
输出：[1,4,2,5,3,8,6,9,7,10,11]
```

示例 4：

```text
输入：nums = [[1,2,3,4,5,6]]
输出：[1,2,3,4,5,6]
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i].length <= 10 ^ 5`
- `1 <= nums[i][j] <= 10 ^ 9`
- `nums` 中最多有 `10 ^ 5` 个数字。

__思路:__

```text
模拟 ➕ 自定义排序
注意到对角线上 i + j 为定值
只需要将 i + j 相同的值放入同一个有序哈希表内即可
或者自定义排序
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> findDiagonalOrder(vector<vector<int>>& nums) 
    {
        map<int,vector<int>> record;
        for (int n = nums.size(), i = n - 1; i > -1; i--) for(int m = nums[i].size(), j = 0; j < m; j++) record[i + j].emplace_back(nums[i][j]);
        vector<int> result;
        for (const auto& [k, v]: record) for (const auto& a: v) result.emplace_back(a);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] findDiagonalOrder(List<List<Integer>> nums) {
        List<int[]> list = new ArrayList<>();
        for (int i = 0, n = nums.size(); i < n; i++) for (int m = nums.get(i).size(), j = 0; j < m; j++) list.add(new int[]{nums.get(i).get(j), i, j});
        list.sort(new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return o1[1] + o1[2] - o2[1] - o2[2] == 0 ? (o1[2] - o2[2]) : o1[1] + o1[2] - o2[1] - o2[2];
            }
        });
        int p = list.size(), result[] = new int[p];
        for (int i = 0; i < p; i++) result[i] = list.get(i)[0];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findDiagonalOrder(self, nums: List[List[int]]) -> List[int]:
        result, n = deque(), len(nums)
        for i in range(n):
            for j in range(len(nums[i])):
                result.append((i + j, j, nums[i][j]))
        return [i[2] for i in sorted(result)]
```
