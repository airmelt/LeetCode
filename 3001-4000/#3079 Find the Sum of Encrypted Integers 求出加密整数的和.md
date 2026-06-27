# 3079 Find the Sum of Encrypted Integers 求出加密整数的和

__Description:__

You are given an integer array `nums` containing __positive__ integers. We define a function `encrypt` such that `encrypt(x)` replaces __every__ digit in `x` with the __largest__ digit in `x`. For example, `encrypt(523) = 555` and `encrypt(213) = 333`.

Return the sum of encrypted elements.

__Example:__

Example 1:

```text
Input: nums = [1,2,3]

Output: 6
```

__Explanation:__ The encrypted elements are `[1,2,3]`. The sum of encrypted elements is `1 + 2 + 3 == 6`.

Example 2:

```text
Input: nums = [10,21,31]

Output: 66
```

__Explanation:__ The encrypted elements are `[11,22,33]`. The sum of encrypted elements is `11 + 22 + 33 == 66`.

__Constraints:__

- `1 <= nums.length <= 50`
- `1 <= nums[i] <= 1000`

__题目描述:__

给你一个整数数组 `nums` ，数组中的元素都是 __正__ 整数。定义一个加密函数 `encrypt` ， `encrypt(x)` 将一个整数 `x` 中 __每一个__ 数位都用 `x` 中的 __最大__ 数位替换。比方说 `encrypt(523) = 555` 且 `encrypt(213) = 333` 。

请你返回数组中所有元素加密后的 和 。

__示例:__

示例 1：

```text
输入：nums = [1,2,3]

输出：6
```

_解释:_ 加密后的元素位 `[1,2,3]` 。加密元素的和为 `1 + 2 + 3 == 6` 。

示例 2：

```text
输入：nums = [10,21,31]

输出：66
```

_解释:_ 加密后的元素为 `[11,22,33]` 。加密元素的和为 `11 + 22 + 33 == 66` 。

__提示：__

- `1 <= nums.length <= 50`
- `1 <= nums[i] <= 1000`

__思路:__

```text
1. 暴力法
将数字转换为字符串
找到最大值乘上字符串长度
时间复杂度为 O(NlogM), 空间复杂度为 O(logM), M 为数组元素的最大值
2. 数学
计算数字模 10 的最大值
同时将数字转化为 111...
最后使用乘法并累加结果
时间复杂度为 O(NlogM), 空间复杂度为 O(1), M 为数组元素的最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int sumOfEncryptedInt(vector<int>& nums) 
    {
        int result = 0;
        for (const auto& x : nums) 
        {
            int mx = 0, base = 0, num = x;
            for (; num > 0; num /= 10) 
            {
                mx = max(mx, num % 10);
                base = 10 * base + 1;
            }
            result += mx * base;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int sumOfEncryptedInt(int[] nums) {
        int result = 0;
        for (int num : nums) {
            int mx = 0, base = 0;
            for (; num > 0; num /= 10) {
                mx = Math.max(mx, num % 10);
                base = 10 * base + 1;
            }
            result += mx * base;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def sumOfEncryptedInt(self, nums: List[int]) -> int:
        return sum(int(max(str(i)) * len(str(i))) for i in nums)
```
