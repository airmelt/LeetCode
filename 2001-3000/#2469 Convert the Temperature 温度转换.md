# 2469 Convert the Temperature 温度转换

__Description:__

You are given a non-negative floating point number rounded to two decimal places `celsius`, that denotes the __temperature in Celsius__.

You should convert Celsius into __Kelvin__ and __Fahrenheit__ and return it as an array `ans = [kelvin, fahrenheit]`.

Return _the array `ans`._ Answers within `10 ^ -5` of the actual answer will be accepted.

Note that:

- `Kelvin = Celsius + 273.15`
- `Fahrenheit = Celsius * 1.80 + 32.00`

__Example:__

Example 1:

```text
Input: celsius = 36.50
Output: [309.65000,97.70000]
Explanation: Temperature at 36.50 Celsius converted in Kelvin is 309.65 and converted in Fahrenheit is 97.70.
```

Example 2:

```text
Input: celsius = 122.11
Output: [395.26000,251.79800]
Explanation: Temperature at 122.11 Celsius converted in Kelvin is 395.26 and converted in Fahrenheit is 251.798.
```

__Constraints:__

- `0 <= celsius <= 1000`

__题目描述:__

给你一个四舍五入到两位小数的非负浮点数 `celsius` 来表示温度，以 __摄氏度__（__Celsius__）为单位。

你需要将摄氏度转换为 __开氏度__（__Kelvin__）和 __华氏度__（__Fahrenheit__），并以数组 `ans = [kelvin, fahrenheit]` 的形式返回结果。

返回数组 _`ans`_ 。与实际答案误差不超过 `10 ^ -5` 的会视为正确答案。

注意：

- `开氏度 = 摄氏度 + 273.15`
- `华氏度 = 摄氏度 * 1.80 + 32.00`

__示例:__

示例 1 ：

```text
输入：celsius = 36.50
输出：[309.65000,97.70000]
解释：36.50 摄氏度：转换为开氏度是 309.65 ，转换为华氏度是 97.70 。
```

示例 2 ：

```text
输入：celsius = 122.11
输出：[395.26000,251.79800]
解释：122.11 摄氏度：转换为开氏度是 395.26 ，转换为华氏度是 251.798 。
```

__提示：__

- `0 <= celsius <= 1000`

__思路:__

```text
模拟
按照题目给出的公式进行计算即可
开氏度为摄氏度 + 273.15
华氏度为摄氏度 * 1.80 + 32.00
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<double> convertTemperature(double celsius) 
    {
        return {celsius + 273.15, celsius * 1.8 + 32};
    }
};
```

__Java__:

```Java
class Solution {
    public double[] convertTemperature(double celsius) {
        return new double[]{celsius + 273.15, celsius * 1.8 + 32};
    }
}
```

__Python__:

```Python
class Solution:
    def convertTemperature(self, celsius: float) -> List[float]:
        return [celsius + 273.15, celsius * 1.8 + 32]
```
