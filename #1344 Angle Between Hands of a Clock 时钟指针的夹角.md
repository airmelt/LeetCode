# 1344 Angle Between Hands of a Clock 时钟指针的夹角

__Description:__

Given two numbers, hour and minutes, return the smaller angle (in degrees) formed between the hour and the minute hand.

Answers within 10^-5 of the actual value will be accepted as correct.

__Example:__

Example 1:

[图片上传失败...(image-4a27e-1665736537538)]

Input: hour = 12, minutes = 30
Output: 165

Example 2:

[图片上传失败...(image-ca5a12-1665736537539)]

Input: hour = 3, minutes = 30
Output: 75

Example 3:

[图片上传失败...(image-35a6ef-1665736537539)]

Input: hour = 3, minutes = 15
Output: 7.5

__Constraints:__

1 <= hour <= 12
0 <= minutes <= 59

__题目描述：__

给你两个数 hour 和 minutes 。请你返回在时钟上，由给定时间的时针和分针组成的较小角的角度（60 单位制）。

__示例：__

示例 1：

[图片上传失败...(image-b646f4-1665736537539)]

输入：hour = 12, minutes = 30
输出：165

示例 2：

[图片上传失败...(image-80c538-1665736537539)]

输入：hour = 3, minutes = 30
输出；75

示例 3：

[图片上传失败...(image-961a19-1665736537539)]

输入：hour = 3, minutes = 15
输出：7.5

示例 4：

输入：hour = 4, minutes = 50
输出：155

示例 5：

输入：hour = 12, minutes = 0
输出：0

__提示：__

1 <= hour <= 12
0 <= minutes <= 59
与标准答案误差在 10^-5 以内的结果都被视为正确结果。

__思路：__

数学
追及问题
时针 12 小时走一圈, 1 小时走 30 度, 1 分钟走 0.5 度
分针 1 小时走一圈, 1 分钟走 6 度
那么分针每分钟要比时针多走 5.5 度
所以 r = hour \* 30 - miunutes \* 5.5 就是时针和分针的夹角
返回 r 和 360 - r 中较小值保证返回 0-180 度的结果
时间复杂度为 O(1), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    double angleClock(int hour, int minutes) 
    {
        return min(abs(hour * 30 - minutes * 5.5), 360.0 - abs(hour * 30 - minutes * 5.5));
    }
};
```

__Java__:

```Java
class Solution {
    public double angleClock(int hour, int minutes) {
        return Math.min(Math.abs(hour * 30 - minutes * 5.5), 360.0 - Math.abs(hour * 30 - minutes * 5.5));
    }
}
```

__Python__:

```Python
class Solution:
    def angleClock(self, hour: int, minutes: int) -> float:
        return min((r := abs(hour * 30 - minutes * 5.5)), 360 - r)
```
