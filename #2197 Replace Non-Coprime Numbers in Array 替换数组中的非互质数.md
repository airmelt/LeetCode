# 2197 Replace Non-Coprime Numbers in Array 替换数组中的非互质数

__Description:__

You are given an array of integers `nums`. Perform the following steps:

Return the final modified array. It can be shown that replacing adjacent non-coprime numbers in any arbitrary order will lead to the same result.

The test cases are generated such that the values in the final array are __less than or equal__ to `10 ^ 8`.

Two values `x` and `y` are __non-coprime__ if `GCD(x, y) > 1` where `GCD(x, y)` is the __Greatest Common Divisor__ of `x` and `y`.

__Example:__

Example 1:

```text
Input: nums = [6,4,3,2,7,6,2]
Output: [12,7,6]
Explanation: 
```

- (6, 4) are non-coprime with LCM(6, 4) = 12. Now, nums = [12,3,2,7,6,2].
- (12, 3) are non-coprime with LCM(12, 3) = 12. Now, nums = [12,2,7,6,2].
- (12, 2) are non-coprime with LCM(12, 2) = 12. Now, nums = [12,7,6,2].
- (6, 2) are non-coprime with LCM(6, 2) = 6. Now, nums = [12,7,6].

There are no more adjacent non-coprime numbers in nums.
Thus, the final modified array is [12,7,6].
Note that there are other ways to obtain the same resultant array.

Example 2:

```text
Input: nums = [2,2,1,1,3,3,3]
Output: [2,1,1,3]
Explanation: 
```

- (3, 3) are non-coprime with LCM(3, 3) = 3. Now, nums = [2,2,1,1,3,3].
- (3, 3) are non-coprime with LCM(3, 3) = 3. Now, nums = [2,2,1,1,3].
- (2, 2) are non-coprime with LCM(2, 2) = 2. Now, nums = [2,1,1,3].

There are no more adjacent non-coprime numbers in nums.
Thus, the final modified array is [2,1,1,3].
Note that there are other ways to obtain the same resultant array.

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`
- The test cases are generated such that the values in the final array are __less than or equal__ to `10 ^ 8`.

__题目描述:__

给你一个整数数组 `nums` 。请你对数组执行下述操作:

返回修改后得到的 最终 数组。可以证明的是，以 任意 顺序替换相邻的非互质数都可以得到相同的结果。

生成的测试用例可以保证最终数组中的值 __小于或者等于__ `10 ^ 8` 。

两个数字 `x` 和 `y` 满足 __非互质数__ 的条件是: `GCD(x, y) > 1` ，其中 `GCD(x, y)` 是 `x` 和 `y` 的 __最大公约数__ 。

__示例:__

示例 1 ：

```text
输入：nums = [6,4,3,2,7,6,2]
输出：[12,7,6]
解释：
```

- (6, 4) 是一组非互质数，且 LCM(6, 4) = 12 。得到 nums = [12,3,2,7,6,2] 。
- (12, 3) 是一组非互质数，且 LCM(12, 3) = 12 。得到 nums = [12,2,7,6,2] 。
- (12, 2) 是一组非互质数，且 LCM(12, 2) = 12 。得到 nums = [12,7,6,2] 。
- (6, 2) 是一组非互质数，且 LCM(6, 2) = 6 。得到 nums = [12,7,6] 。

现在，nums 中不存在相邻的非互质数。
因此，修改后得到的最终数组是 [12,7,6] 。
注意，存在其他方法可以获得相同的最终数组。

示例 2 ：

```text
输入：nums = [2,2,1,1,3,3,3]
输出：[2,1,1,3]
解释：
```

- (3, 3) 是一组非互质数，且 LCM(3, 3) = 3 。得到 nums = [2,2,1,1,3,3] 。
- (3, 3) 是一组非互质数，且 LCM(3, 3) = 3 。得到 nums = [2,2,1,1,3] 。
- (2, 2) 是一组非互质数，且 LCM(2, 2) = 2 。得到 nums = [2,1,1,3] 。

现在，nums 中不存在相邻的非互质数。
因此，修改后得到的最终数组是 [2,1,1,3] 。
注意，存在其他方法可以获得相同的最终数组。

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`
- 生成的测试用例可以保证最终数组中的值 __小于或者等于__ `10 ^ 8` 。

__思路:__

```text
数学
按照题目提示, 两个数选择的顺序不影响最终结果
那么每次我们直接取最后两个数计算即可
遍历 nums 每次加入一个数
如果结果数组的 size 比 1 大, 则计算最后两个数的最大公约数 g
如果 g == 1, 则退出循环
否则将最后一个数 pop 出来, 并将倒数第二个数乘以最后一个数除以 g
时间复杂度为 O(NlogC), 空间复杂度为 O(1), C 为数组中的最大值, 主要是计算最大公约数的时间复杂度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> replaceNonCoprimes(vector<int>& nums) 
    {
        vector<int> result;
        for (const auto& num : nums) 
        {
            result.emplace_back(num);
            while (result.size() > 1) 
            {
                long long x = result.back(), y = result[result.size() - 2], g = gcd(x, y);
                if (g == 1LL) break;
                result.pop_back();
                result.back() *= x / g;
            }
        }
        return result;  
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> replaceNonCoprimes(int[] nums) {
        List<Integer> result = new ArrayList<>();
        for (int num : nums) {
            result.add(num);
            while (result.size() > 1) {
                long x = result.get(result.size() - 1), y = result.get(result.size() - 2), g = gcd(x, y);
                if (g == 1L) break;
                result.remove(result.size() - 1);
                result.set(result.size() - 1, (int)(result.get(result.size() - 1) * x / g));
            }
        }
        return result;
    }

    private long gcd(long a, long b) {
        return b == 0L ? a : gcd(b, a % b);
    }
}
```

__Python__:

```Python
class Solution:
    def replaceNonCoprimes(self, nums: List[int]) -> List[int]:
        result = []
        for num in nums:
            result.append(num)
            while len(result) > 1:
                x, y = result[-1], result[-2]
                if (g := gcd(x, y)) == 1:
                    break
                result.pop()
                result[-1] *= x // g
        return result
```
