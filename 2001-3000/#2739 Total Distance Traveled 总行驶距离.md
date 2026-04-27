# 2739 Total Distance Traveled 总行驶距离

__Description:__

A truck has two fuel tanks. You are given two integers, `mainTank` representing the fuel present in the main tank in liters and `additionalTank` representing the fuel present in the additional tank in liters.

The truck has a mileage of `10` km per liter. Whenever `5` liters of fuel get used up in the main tank, if the additional tank has at least `1` liters of fuel, `1` liters of fuel will be transferred from the additional tank to the main tank.

Return the maximum distance which can be traveled.

Note: Injection from the additional tank is not continuous. It happens suddenly and immediately for every 5 liters consumed.

__Example:__

Example 1:

```text
Input: mainTank = 5, additionalTank = 10
Output: 60
Explanation: 
After spending 5 litre of fuel, fuel remaining is (5 - 5 + 1) = 1 litre and distance traveled is 50km.
After spending another 1 litre of fuel, no fuel gets injected in the main tank and the main tank becomes empty.
Total distance traveled is 60km.
```

Example 2:

```text
Input: mainTank = 1, additionalTank = 2
Output: 10
Explanation: 
After spending 1 litre of fuel, the main tank becomes empty.
Total distance traveled is 10km.
```

__Constraints:__

- `1 <= mainTank, additionalTank <= 100`

__题目描述:__

卡车有两个油箱。给你两个整数， `mainTank` 表示主油箱中的燃料（以升为单位）， `additionalTank` 表示副油箱中的燃料（以升为单位）。

该卡车每耗费 `1` 升燃料都可以行驶 `10` km。每当主油箱使用了 `5` 升燃料时，如果副油箱至少有 `1` 升燃料，则会将 `1` 升燃料从副油箱转移到主油箱。

返回卡车可以行驶的最大距离。

注意:从副油箱向主油箱注入燃料不是连续行为。这一事件会在每消耗 `5` 升燃料时突然且立即发生。

__示例:__

示例 1：

```text
输入：mainTank = 5, additionalTank = 10
输出：60
解释：
在用掉 5 升燃料后，主油箱中燃料还剩下 (5 - 5 + 1) = 1 升，行驶距离为 50km 。
在用掉剩下的 1 升燃料后，没有新的燃料注入到主油箱中，主油箱变为空。
总行驶距离为 60km 。
```

示例 2：

```text
输入：mainTank = 1, additionalTank = 2
输出：10
解释：
在用掉 1 升燃料后，主油箱变为空。
总行驶距离为 10km 。
```

__提示：__

- `1 <= mainTank, additionalTank <= 100`

__思路:__

```text
数学
主油箱每 4L 燃料就能再加一次副邮箱的油
所以取两者的较小值得到最多能加入的油
主油箱的油一定能用完
最后再乘以 10 即可
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int distanceTraveled(int mainTank, int additionalTank) 
    {
        return (mainTank + (min(additionalTank, (mainTank - 1) >> 2))) * 10;
    }
};
```

__Java__:

```Java
class Solution {
    public int distanceTraveled(int mainTank, int additionalTank) {
        return (mainTank + (Math.min(additionalTank, (mainTank - 1) >>> 2))) * 10;
    }
}
```

__Python__:

```Python
class Solution:
    def distanceTraveled(self, mainTank: int, additionalTank: int) -> int:
        return (mainTank + (min(additionalTank, (mainTank - 1) >> 2))) * 10
```
