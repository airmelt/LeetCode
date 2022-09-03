# 1291 Sequential Digits 顺次数

__Description:__

An integer has sequential digits if and only if each digit in the number is one more than the previous digit.

Return a sorted list of all the integers in the range [low, high] inclusive that have sequential digits.

__Example:__

Example 1:

Input: low = 100, high = 300
Output: [123,234]

Example 2:

Input: low = 1000, high = 13000
Output: [1234,2345,3456,4567,5678,6789,12345]

__Constraints:__

10 <= low <= high <= 10^9

__题目描述：__

我们定义「顺次数」为：每一位上的数字都比前一位上的数字大 1 的整数。

请你返回由 [low, high] 范围内所有顺次数组成的 有序 列表（从小到大排序）。

__示例：__

示例 1：

输出：low = 100, high = 300
输出：[123,234]

示例 2：

输出：low = 1000, high = 13000
输出：[1234,2345,3456,4567,5678,6789,12345]

__提示：__

10 <= low <= high <= 10^9

__思路：__

模拟
因为最多只需要枚举 10 * 10 个数字
简单枚举并排序, 或者先预计算出所有顺次数再筛选
时间复杂度为 O(1), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    vector<int> sequentialDigits(int low, int high) 
    {
        vector<int> result;
        for (int i = 1; i <= 9; i++)  for (int num = i, j = i + 1; j <= 9; j++) if (((num = num * 10 + j) >= low) and num <= high) result.emplace_back(num);
        sort(result.begin(), result.end());
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> sequentialDigits(int low, int high) {
        return Arrays.stream(new int[]{12, 23, 34, 45, 56, 67, 78, 89, 123, 234, 345, 456, 567, 678, 789, 1234, 2345, 3456,4567, 5678, 6789, 12345, 23456, 34567, 45678, 56789, 123456, 234567, 345678, 456789, 1234567, 2345678, 3456789, 12345678, 23456789, 123456789}).boxed().filter(i -> i >= low && i <= high).collect(Collectors.toList());
    }
}
```

__Python__:

```Python
class Solution:
    def sequentialDigits(self, low: int, high: int) -> List[int]:
        return [int(''.join([str(k) for k in range(j, j + i)])) for i in range(len(str(low)), len(str(high)) + 1) for j in range(1, 11 - i) if int(''.join([str(k) for k in range(j, j + i)])) >= low and int(''.join([str(k) for k in range(j, j + i)])) <= high]
```
