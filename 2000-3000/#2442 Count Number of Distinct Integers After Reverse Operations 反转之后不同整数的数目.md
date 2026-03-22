# 2442 Count Number of Distinct Integers After Reverse Operations 反转之后不同整数的数目

__Description:__

You are given an array `nums` consisting of __positive__ integers.

You have to take each integer in the array, __reverse its digits__, and add it to the end of the array. You should apply this operation to the original integers in `nums`.

Return the number of distinct integers in the final array.

__Example:__

Example 1:

```text
Input: nums = [1,13,10,12,31]
Output: 6
Explanation: After including the reverse of each number, the resulting array is [1,13,10,12,31,1,31,1,21,13].
The reversed integers that were added to the end of the array are underlined. Note that for the integer 10, after reversing it, it becomes 01 which is just 1.
The number of distinct integers in this array is 6 (The numbers 1, 10, 12, 13, 21, and 31).
```

Example 2:

```text
Input: nums = [2,2,2]
Output: 1
Explanation: After including the reverse of each number, the resulting array is [2,2,2,2,2,2].
The number of distinct integers in this array is 1 (The number 2).
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`

__题目描述:__

给你一个由 __正__ 整数组成的数组 `nums` 。

你必须取出数组中的每个整数，__反转其中每个数位__，并将反转后得到的数字添加到数组的末尾。这一操作只针对 `nums` 中原有的整数执行。

返回结果数组中 不同 整数的数目。

__示例:__

示例 1：

```text
输入：nums = [1,13,10,12,31]
输出：6
解释：反转每个数字后，结果数组是 [1,13,10,12,31,1,31,1,21,13] 。
反转后得到的数字添加到数组的末尾并按斜体加粗表示。注意对于整数 10 ，反转之后会变成 01 ，即 1 。
数组中不同整数的数目为 6（数字 1、10、12、13、21 和 31）。
```

示例 2：

```text
输入：nums = [2,2,2]
输出：1
解释：反转每个数字后，结果数组是 [2,2,2,2,2,2] 。
数组中不同整数的数目为 1（数字 2）。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`

__思路:__

```text
哈希表
将反转之后的元素和原元素一起加入哈希表中
最后返回哈希表的大小即可
时间复杂度为 O(NlogM), 空间复杂度为 O(N), 其中 N 为数组长度, M 为数组中元素的最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countDistinctIntegers(vector<int>& nums) 
    {
        unordered_set<int> s;
        for (auto num : nums) 
        {
            s.insert(num);
            int cur = 0;
            while (num) 
            {
                cur = 10 * cur + num % 10;
                num /= 10;
            }
            s.insert(cur);
        }
        return s.size();
    }
};
```

__Java__:

```Java
class Solution {
    public int countDistinctIntegers(int[] nums) {
        var set = new HashSet<Integer>();
        for (int num : nums) {
            set.add(num);
            int cur = 0;
            while (num > 0) {
                cur = 10 * cur + num % 10;
                num /= 10;
            }
            set.add(cur);
        }
        return set.size();
    }
}
```

__Python__:

```Python
class Solution:
    def countDistinctIntegers(self, nums: List[int]) -> int:
        return len(set(int(str(num)[::-1]) for num in nums) | set(nums))
```
