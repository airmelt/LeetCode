# 2216 Minimum Deletions to Make Array Beautiful 美化数组的最少删除数

__Description:__

You are given a __0-indexed__ integer array `nums`. The array `nums` is __beautiful__ if:

- `nums.length` is even.
- `nums[i] != nums[i + 1]` for all `i % 2 == 0`.

Note that an empty array is considered beautiful.

You can delete any number of elements from `nums`. When you delete an element, all the elements to the right of the deleted element will be __shifted one unit to the left__ to fill the gap created and all the elements to the left of the deleted element will remain __unchanged__.

Return _the __minimum__ number of elements to delete from_ `nums` _to make it_ _beautiful._

__Example:__

Example 1:

```text
Input:  nums = [1,1,2,3,5]
Output:  1
Explanation:  You can delete either `nums[0]` or `nums[1]` to make `nums` = [1,2,3,5] which is beautiful. It can be proven you need at least 1 deletion to make `nums` beautiful.
```

Example 2:

```text
Input:  nums = [1,1,2,2,3,3]
Output:  2
Explanation:  You can delete `nums[0]` and `nums[5]` to make nums = [1,2,2,3] which is beautiful. It can be proven you need at least 2 deletions to make nums beautiful.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，如果满足下述条件，则认为数组 `nums` 是一个 __美丽数组__ :

- `nums.length` 为偶数
- 对所有满足 `i % 2 == 0` 的下标 `i` ， `nums[i] != nums[i + 1]` 均成立

注意，空数组同样认为是美丽数组。

你可以从 `nums` 中删除任意数量的元素。当你删除一个元素时，被删除元素右侧的所有元素将会向左移动一个单位以填补空缺，而左侧的元素将会保持 __不变__ 。

返回使 `nums` 变为美丽数组所需删除的 __最少__ 元素数目_。_

__示例:__

示例 1：

```text
输入: nums = [1,1,2,3,5]
输出: 1
解释: 可以删除 `nums[0]` 或 `nums[1]` ，这样得到的 `nums` = [1,2,3,5] 是一个美丽数组。可以证明，要想使 nums 变为美丽数组，至少需要删除 1 个元素。
```

示例 2：

```text
输入: nums = [1,1,2,2,3,3]
输出: 2
解释: 可以删除 `nums[0]` 和 `nums[5]` ，这样得到的 nums = [1,2,2,3] 是一个美丽数组。可以证明，要想使 nums 变为美丽数组，至少需要删除 2 个元素。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 5`

__思路:__

```text
模拟
遍历数组, 直到倒数第二个元素
如果删除了一个元素, 则新的下标应该为 i - result
所以需要比较 (i - result) & 1 是否为 0, 且 nums[i] == nums[i + 1]
如果为真, 则 result 自增
最后一个元素需要特殊处理, 如果 n - result 为奇数, 则需要删除最后一个元素
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minDeletion(vector<int>& nums) 
    {
        int n = nums.size(), result = 0;
        for (int i = 0; i < n - 1; i++) result += (!((i - result) & 1) and nums[i] == nums[i + 1]);
        return result + ((n - result) & 1);
    }
};
```

__Java__:

```Java
class Solution {
    public int minDeletion(int[] nums) {
        int n = nums.length, result = 0;
        for (int i = 0; i < n - 1; i++) if (((i - result) & 1) == 0 && nums[i] == nums[i + 1]) ++result;
        return result + (((n - result) & 1) == 1 ? 1 : 0);
    }
}
```

__Python__:

```Python
class Solution:
    def minDeletion(self, nums: List[int]) -> int:
        n, result = len(nums), 0
        for i in range(n - 1):
            result += not ((i - result) & 1) and nums[i] == nums[i + 1]
        return result + ((n - result) & 1)
```
