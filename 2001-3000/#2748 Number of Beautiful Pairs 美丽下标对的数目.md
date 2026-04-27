# 2748 Number of Beautiful Pairs 美丽下标对的数目

__Description:__

You are given a __0-indexed__ integer array `nums`. A pair of indices `i`, `j` where `0 <= i < j < nums.length` is called beautiful if the __first digit__ of `nums[i]` and the __last digit__ of `nums[j]` are __coprime__.

Return _the total number of beautiful pairs in_ `nums`.

Two integers `x` and `y` are __coprime__ if there is no integer greater than 1 that divides both of them. In other words, `x` and `y` are coprime if `gcd(x, y) == 1`, where `gcd(x, y)` is the __greatest common divisor__ of `x` and `y`.

__Example:__

Example 1:

```text
Input: nums = [2,5,1,4]
Output: 5
Explanation: There are 5 beautiful pairs in nums:
When i = 0 and j = 1: the first digit of nums[0] is 2, and the last digit of nums[1] is 5. We can confirm that 2 and 5 are coprime, since gcd(2,5) == 1.
When i = 0 and j = 2: the first digit of nums[0] is 2, and the last digit of nums[2] is 1. Indeed, gcd(2,1) == 1.
When i = 1 and j = 2: the first digit of nums[1] is 5, and the last digit of nums[2] is 1. Indeed, gcd(5,1) == 1.
When i = 1 and j = 3: the first digit of nums[1] is 5, and the last digit of nums[3] is 4. Indeed, gcd(5,4) == 1.
When i = 2 and j = 3: the first digit of nums[2] is 1, and the last digit of nums[3] is 4. Indeed, gcd(1,4) == 1.
Thus, we return 5.
```

Example 2:

```text
Input: nums = [11,21,12]
Output: 2
Explanation: There are 2 beautiful pairs:
When i = 0 and j = 1: the first digit of nums[0] is 1, and the last digit of nums[1] is 1. Indeed, gcd(1,1) == 1.
When i = 0 and j = 2: the first digit of nums[0] is 1, and the last digit of nums[2] is 2. Indeed, gcd(1,2) == 1.
Thus, we return 2.
```

__Constraints:__

- `2 <= nums.length <= 100`
- `1 <= nums[i] <= 9999`
- `nums[i] % 10 != 0`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。如果下标对 `i`、 `j` 满足 `0 ≤ i < j < nums.length` ，如果 `nums[i]` 的 __第一个数字__ 和 `nums[j]` 的 __最后一个数字__ __互质__ ，则认为 `nums[i]` 和 `nums[j]` 是一组 __美丽下标对__ 。

返回 `nums` 中 __美丽下标对__ 的总数目。

对于两个整数 `x` 和 `y` ，如果不存在大于 1 的整数可以整除它们，则认为 `x` 和 `y` __互质__ 。换而言之，如果 `gcd(x, y) == 1` ，则认为 `x` 和 `y` 互质，其中 `gcd(x, y)` 是 `x` 和 `y` 的 __最大公因数__ 。

__示例:__

示例 1：

```text
输入：nums = [2,5,1,4]
输出：5
解释：nums 中共有 5 组美丽下标对：
i = 0 和 j = 1 ：nums[0] 的第一个数字是 2 ，nums[1] 的最后一个数字是 5 。2 和 5 互质，因此 gcd(2,5) == 1 。
i = 0 和 j = 2 ：nums[0] 的第一个数字是 2 ，nums[2] 的最后一个数字是 1 。2 和 1 互质，因此 gcd(2,1) == 1 。
i = 1 和 j = 2 ：nums[1] 的第一个数字是 5 ，nums[2] 的最后一个数字是 1 。5 和 1 互质，因此 gcd(5,1) == 1 。
i = 1 和 j = 3 ：nums[1] 的第一个数字是 5 ，nums[3] 的最后一个数字是 4 。5 和 4 互质，因此 gcd(5,4) == 1 。
i = 2 和 j = 3 ：nums[2] 的第一个数字是 1 ，nums[3] 的最后一个数字是 4 。1 和 4 互质，因此 gcd(1,4) == 1 。
因此，返回 5 。
```

示例 2：

```text
输入：nums = [11,21,12]
输出：2
解释：共有 2 组美丽下标对：
i = 0 和 j = 1 ：nums[0] 的第一个数字是 1 ，nums[1] 的最后一个数字是 1 。gcd(1,1) == 1 。
i = 0 和 j = 2 ：nums[0] 的第一个数字是 1 ，nums[2] 的最后一个数字是 2 。gcd(1,2) == 1 。
因此，返回 2 。
```

__提示：__

- `2 <= nums.length <= 100`
- `1 <= nums[i] <= 9999`
- `nums[i] % 10 != 0`

__思路:__

```text
1. 暴力法
将 nums[i] 的最高位提取出来
即一直除 10 直到 nums[i] 不大于 10
检查 i 之后每一个数是否能和 nums[i] 的最高位互质
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 计数
nums[i] 的最高位只能是 [1, 9]
用一个长度为 10 的数组记录出现的次数
然后遍历 nums
如果 nums[i] 的最低位和 [1, 9] 中某个数互质
最低位可由 nums[i] % 10 得到
就加上这个数的计数
再把 nums[i] 的最高位加入计数
时间复杂度为 O(CN), 空间复杂度为 O(C), 其中 C = 10(实际上应该为 9, 用 10 处理更方便)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countBeautifulPairs(vector<int>& nums) 
    {
        int result = 0, n = nums.size();
        for (int i = 0; i < n; i++)
        {
            while (nums[i] > 9) nums[i] /= 10;
            for (int j = i + 1; j < n; j++) result += gcd(nums[i], nums[j] % 10) == 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countBeautifulPairs(int[] nums) {
        int result = 0, count[] = new int[10];
        for (int num : nums) {
            for (int i = 1; i < 10; i++) if (count[i] > 0 && gcd(i, num % 10) == 1) result += count[i];
            while (num > 9) num /= 10;
            ++count[num];
        }
        return result;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

__Python__:

```Python
class Solution:
    def countBeautifulPairs(self, nums: List[int]) -> int:
        return sum(gcd(int(str(nums[i])[0]), int(str(nums[j])[-1])) == 1 for i in range(len(nums)) for j in range(i + 1, len(nums)))
```
