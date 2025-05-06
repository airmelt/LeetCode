# 2513 Minimize the Maximum of Two Arrays 最小化两个数组中的最大值

__Description:__

We have two arrays `arr1` and `arr2` which are initially empty. You need to add positive integers to them such that they satisfy all the following conditions:

- `arr1` contains `uniqueCnt1` __distinct__ positive integers, each of which is __not divisible__ by `divisor1`.
- `arr2` contains `uniqueCnt2` __distinct__ positive integers, each of which is __not divisible__ by `divisor2`.
- __No__ integer is present in both `arr1` and `arr2`.

Given `divisor1`, `divisor2`, `uniqueCnt1`, and `uniqueCnt2`, return _the __minimum possible maximum__ integer that can be present in either array_.

__Example:__

Example 1:

```text
Input: divisor1 = 2, divisor2 = 7, uniqueCnt1 = 1, uniqueCnt2 = 3
Output: 4
Explanation: 
We can distribute the first 4 natural numbers into arr1 and arr2.
arr1 = [1] and arr2 = [2,3,4].
We can see that both arrays satisfy all the conditions.
Since the maximum value is 4, we return it.
```

Example 2:

```text
Input: divisor1 = 3, divisor2 = 5, uniqueCnt1 = 2, uniqueCnt2 = 1
Output: 3
Explanation: 
Here arr1 = [1,2], and arr2 = [3] satisfy all conditions.
Since the maximum value is 3, we return it.
```

Example 3:

```text
Input: divisor1 = 2, divisor2 = 4, uniqueCnt1 = 8, uniqueCnt2 = 2
Output: 15
Explanation: 
Here, the final possible arrays can be arr1 = [1,3,5,7,9,11,13,15], and arr2 = [2,6].
It can be shown that it is not possible to obtain a lower maximum satisfying all conditions.
```

__Constraints:__

- `2 <= divisor1, divisor2 <= 10 ^ 5`
- `1 <= uniqueCnt1, uniqueCnt2 < 10 ^ 9`
- `2 <= uniqueCnt1 + uniqueCnt2 <= 10 ^ 9`

__题目描述:__

给你两个数组 `arr1` 和 `arr2` ，它们一开始都是空的。你需要往它们中添加正整数，使它们满足以下条件:

- `arr1` 包含 `uniqueCnt1` 个 __互不相同__ 的正整数，每个整数都 __不能__ 被 `divisor1` __整除__ 。
- `arr2` 包含 `uniqueCnt2` 个 __互不相同__ 的正整数，每个整数都 __不能__ 被 `divisor2` __整除__ 。
- `arr1` 和 `arr2` 中的元素 __互不相同__ 。

给你 `divisor1` ， `divisor2` ， `uniqueCnt1` 和 `uniqueCnt2` ，请你返回两个数组中 __最大元素__ 的 __最小值__ 。

__示例:__

示例 1：

```text
输入：divisor1 = 2, divisor2 = 7, uniqueCnt1 = 1, uniqueCnt2 = 3
输出：4
解释：
我们可以把前 4 个自然数划分到 arr1 和 arr2 中。
arr1 = [1] 和 arr2 = [2,3,4] 。
可以看出两个数组都满足条件。
最大值是 4 ，所以返回 4 。
```

示例 2：

```text
输入：divisor1 = 3, divisor2 = 5, uniqueCnt1 = 2, uniqueCnt2 = 1
输出：3
解释：
arr1 = [1,2] 和 arr2 = [3] 满足所有条件。
最大值是 3 ，所以返回 3 。
```

示例 3：

```text
输入：divisor1 = 2, divisor2 = 4, uniqueCnt1 = 8, uniqueCnt2 = 2
输出：15
解释：
最终数组为 arr1 = [1,3,5,7,9,11,13,15] 和 arr2 = [2,6] 。
上述方案是满足所有条件的最优解。
```

__提示：__

- `2 <= divisor1, divisor2 <= 10 ^ 5`
- `1 <= uniqueCnt1, uniqueCnt2 < 10 ^ 9`
- `2 <= uniqueCnt1 + uniqueCnt2 <= 10 ^ 9`

__思路:__

```text
二分法
设 divisor1 和 divisor2 的最小公倍数为 d
d = divisor1 * divisor2 / gcd(divisor1, divisor2)
当 divisor1 = 2 和 divisor2 = 2 时, 取到最大值 (uniqueCnt1 + uniqueCnt2) * 2 - 1
即二分的右边界(闭)为 (uniqueCnt1 + uniqueCnt2) * 2 - 1
左边界为 0 (开)
每次如果取不到 left = mid + 1, 因为 mid 都取不到, mid + 1 才可能取到
最多可以
将 mid - mid / divisor1 个数放到 arr1 中
将 mid - mid / divisor2 个数放到 arr2 中
总共使用了 mid - mid / d 个数
这三个条件必须都满足才能满足 mid 个数能放满 arr1 和 arr2
时间复杂度为 O(log(D1 + D2) + log(U1 + U2)), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimizeSet(int divisor1, int divisor2, int uniqueCnt1, int uniqueCnt2) 
    {
        long long left = 0LL, right = ((uniqueCnt1 + uniqueCnt2) << 1LL) - 1LL, mid = 0LL, t1 = 0LL, t2 = 0LL, total = 0LL, d = lcm((long long)divisor1, (long long)divisor2);
        while (left < right) 
        {
            mid = left + ((right - left) >> 1LL);
            t1 = mid - mid / divisor1;
            t2 = mid - mid / divisor2;
            total = mid - mid / d;
            if (t1 < uniqueCnt1 or t2 < uniqueCnt2 or total < (uniqueCnt1 + uniqueCnt2)) left = mid + 1LL;
            else right = mid;
         }
        return left;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimizeSet(int divisor1, int divisor2, int uniqueCnt1, int uniqueCnt2) {
        long left = 0L, right = ((uniqueCnt1 + uniqueCnt2) << 1L) - 1L, mid = 0L, t1 = 0L, t2 = 0L, total = 0L, lcm = helper(divisor1, divisor2);
        while (left < right) {
            mid = left + ((right - left) >> 1L);
            t1 = mid - mid / divisor1;
            t2 = mid - mid / divisor2;
            total = mid - mid / lcm;
            if (t1 < uniqueCnt1 || t2 < uniqueCnt2 || total < (uniqueCnt1 + uniqueCnt2)) left = mid + 1L;
            else right = mid;
        }
        return (int)left;
    }

    private long helper(int a, int b) {
        return (long)a * b / gcd(a, b);
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

__Python__:

```Python
class Solution:
    def minimizeSet(self, divisor1: int, divisor2: int, uniqueCnt1: int, uniqueCnt2: int) -> int:
        return bisect_left(range(((uniqueCnt1 + uniqueCnt2) << 1) - 1), True, key=check) if (d := lcm(divisor1, divisor2)) and (check := lambda mid: (mid - mid // divisor1 - mid // divisor2 + mid // d) >= (max(uniqueCnt1 - mid // divisor2 + mid // d, 0) + max(uniqueCnt2 - mid // divisor1 + mid // d, 0))) else 0
```
