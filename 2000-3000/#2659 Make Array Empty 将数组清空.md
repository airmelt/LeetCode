# 2659 Make Array Empty 将数组清空

__Description:__

You are given an integer array `nums` containing __distinct__ numbers, and you can perform the following operations __until the array is empty__:

- If the first element has the __smallest__ value, remove it
- Otherwise, put the first element at the __end__ of the array.

Return _an integer denoting the number of operations it takes to make_ `nums` _empty._

__Example:__

Example 1:

```text
Input: nums = [3,4,-1]
Output: 5
```

Example 2:

```text
Input: nums = [1,2,4,3]
Output: 5
```

Example 3:

```text
Input: nums = [1,2,3]
Output: 3
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`
- All values in `nums` are __distinct__.

__题目描述:__

给你一个包含若干 __互不相同__ 整数的数组 `nums` ，你需要执行以下操作 __直到__ __数组为空__ :

- 如果数组中第一个元素是当前数组中的 __最小值__ ，则删除它。
- 否则，将第一个元素移动到数组的 __末尾__ 。

请你返回需要多少个操作使 `nums` 为空。

__示例:__

示例 1：

```text
输入：nums = [3,4,-1]
输出：5
```

示例 2：

```text
输入：nums = [1,2,4,3]
输出：5
```

示例 3：

```text
输入：nums = [1,2,3]
输出：3
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`
- `nums` 中的元素 __互不相同__ 。

__思路:__

```text
模拟
若数组有序, 则需要删除 n 次
否则
当数组无序的时候
比如 [2, 4, 1, 3]
按照下标排序可得 [2, 0, 3, 1], 这表示下标对应的位置
找到逆序对的位置
第一次需要弹出位置 2 的元素
需要移动 3 次才能在不影响其他元素的相对顺序的情况下弹出
第二次为 (3, 1)
需要移动 1 次才能弹出
所以每次遇到逆序对需要加上 n - k
时间复杂度为 O(NlogN), 空间复杂度为 O(N), 主要是排序
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long countOperationsToEmptyArray(vector<int>& nums) 
    {
        int n = nums.size(), id[n];
        iota(id, id + n, 0);
        sort(id, id + n, [&](int i, int j) { return nums[i] < nums[j]; });
        long long result = n;
        for (int k = 1; k < n; k++) if (id[k] < id[k - 1]) result += n - k;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long countOperationsToEmptyArray(int[] nums) {
        int n = nums.length;
        var id = new Integer[n];
        for (int i = 0; i < n; i++) id[i] = i;
        Arrays.sort(id, (i, j) -> nums[i] - nums[j]);
        long result = n; 
        for (int k = 1; k < n; k++) if (id[k] < id[k - 1]) result += n - k;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countOperationsToEmptyArray(self, nums: List[int]) -> int:
        return 0 if not (idx := sorted(range(len(nums)), key=lambda x: nums[x])) else sum((len(nums) - k) * (i < pre) for k, (pre, i) in enumerate(pairwise(idx), 1)) + len(nums)
```
