# 2562 Find the Array Concatenation Value 找出数组的串联值

__Description:__

You are given a __0-indexed__ integer array `nums`.

The concatenation of two numbers is the number formed by concatenating their numerals.

- For example, the concatenation of `15`, `49` is `1549`.

The __concatenation value__ of `nums` is initially equal to `0`. Perform this operation until `nums` becomes empty:

- If `nums` has a size greater than one, add the value of the concatenation of the first and the last element to the __concatenation value__ of `nums`, and remove those two elements from `nums`. For example, if the `nums` was `[1, 2, 4, 5, 6]`, add 16 to the `concatenation value`.
- If only one element exists in `nums`, add its value to the __concatenation value__ of `nums`, then remove it.

Return _the concatenation value of `nums`_.

__Example:__

Example 1:

```text
Input: nums = [7,52,2,4]
Output: 596
Explanation: Before performing any operation, nums is [7,52,2,4] and concatenation value is 0.
```

- In the first operation:
We pick the first element, 7, and the last element, 4.
Their concatenation is 74, and we add it to the concatenation value, so it becomes equal to 74.
Then we delete them from nums, so nums becomes equal to [52,2].
- In the second operation:
We pick the first element, 52, and the last element, 2.
Their concatenation is 522, and we add it to the concatenation value, so it becomes equal to 596.
Then we delete them from the nums, so nums becomes empty.
Since the concatenation value is 596 so the answer is 596.

Example 2:

```text
Input: nums = [5,14,13,8,12]
Output: 673
Explanation: Before performing any operation, nums is [5,14,13,8,12] and concatenation value is 0.
```

- In the first operation:
We pick the first element, 5, and the last element, 12.
Their concatenation is 512, and we add it to the concatenation value, so it becomes equal to 512.
Then we delete them from the nums, so nums becomes equal to [14,13,8].
- In the second operation:
We pick the first element, 14, and the last element, 8.
Their concatenation is 148, and we add it to the concatenation value, so it becomes equal to 660.
Then we delete them from the nums, so nums becomes equal to [13].
- In the third operation:
nums has only one element, so we pick 13 and add it to the concatenation value, so it becomes equal to 673.
Then we delete it from nums, so nums become empty.
Since the concatenation value is 673 so the answer is 673.

__Constraints:__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 10 ^ 4`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。

现定义两个数字的 串联 是由这两个数值串联起来形成的新数字。

- 例如， `15` 和 `49` 的串联是 `1549` 。

`nums` 的 __串联值__ 最初等于 `0` 。执行下述操作直到 `nums` 变为空:

- 如果 `nums` 的长度大于 1，分别选中 `nums` 中的第一个元素和最后一个元素，将二者串联得到的值加到 `nums` 的 __串联值__ 上，然后从 `nums` 中删除第一个和最后一个元素。例如，如果 `nums` 是 `[1, 2, 4, 5, 6]`，将 16 添加到串联值。
- 如果 `nums` 中仅存在一个元素，则将该元素的值加到 `nums` 的串联值上，然后删除这个元素。

返回执行完所有操作后 `nums` 的串联值。

__示例:__

示例 1：

```text
输入：nums = [7,52,2,4]
输出：596
解释：在执行任一步操作前，nums 为 [7,52,2,4] ，串联值为 0 。
```

- 在第一步操作中：
我们选中第一个元素 7 和最后一个元素 4 。
二者的串联是 74 ，将其加到串联值上，所以串联值等于 74 。
接着我们从 nums 中移除这两个元素，所以 nums 变为 [52,2] 。
- 在第二步操作中：
我们选中第一个元素 52 和最后一个元素 2 。
二者的串联是 522 ，将其加到串联值上，所以串联值等于 596 。
接着我们从 nums 中移除这两个元素，所以 nums 变为空。
由于串联值等于 596 ，所以答案就是 596 。

示例 2：

```text
输入：nums = [5,14,13,8,12]
输出：673
解释：在执行任一步操作前，nums 为 [5,14,13,8,12] ，串联值为 0 。 
```

- 在第一步操作中：
我们选中第一个元素 5 和最后一个元素 12 。
二者的串联是 512 ，将其加到串联值上，所以串联值等于 512 。
接着我们从 nums 中移除这两个元素，所以 nums 变为 [14,13,8] 。
- 在第二步操作中：
我们选中第一个元素 14 和最后一个元素 8 。
二者的串联是 148 ，将其加到串联值上，所以串联值等于 660 。
接着我们从 nums 中移除这两个元素，所以 nums 变为 [13] 。
- 在第三步操作中：
nums 只有一个元素，所以我们选中 13 并将其加到串联值上，所以串联值等于 673 。
接着我们从 nums 中移除这个元素，所以 nums 变为空。
由于串联值等于 673 ，所以答案就是 673 。

__提示：__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 10 ^ 4`

__思路:__

```text
模拟
使用双指针 i 和 j 分别指向数组的首尾
每次将 nums[i] 和 nums[j] 的串联值加到结果中
然后 i++ 和 j-- 直到 i >= j
如果 i == j，说明数组中只剩一个元素，将其值加到结果中
可以计算 nums[j] 的位数，将 nums[i] 乘以 10 的位数次方后加到结果中
时间复杂度为 O(NlogM), 空间复杂度为 O(1), M 为 nums 中的最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long findTheArrayConcVal(vector<int>& nums) 
    {
        long long result = 0LL;
        int i = 0, j = nums.size() - 1, cur = 0;
        while (i < j) 
        {
            cur = nums[j];
            while (cur) {
                nums[i] *= 10;
                cur /= 10;
            }
            result += nums[i++] + nums[j--];
        }
        return result + (i == j) * nums[i];
    }
};
```

__Java__:

```Java
class Solution {
    public long findTheArrayConcVal(int[] nums) {
        long result = 0L, i = 0L, j = nums.length - 1L;
        for (; i < j; i++, j--) result += Integer.parseInt(nums[(int)i] + "" + nums[(int)j]);
        return result + (i == j ? nums[(int)i] : 0);
    }
}
```

__Python__:

```Python
class Solution:
    def findTheArrayConcVal(self, nums: List[int]) -> int:
        return 0 if not nums else nums[0] if len(nums) == 1 else int(str(nums[0]) + str(nums[-1])) + self.findTheArrayConcVal(nums[1:-1])
```
