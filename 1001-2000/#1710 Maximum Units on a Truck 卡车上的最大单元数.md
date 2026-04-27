# 1710 Maximum Units on a Truck 卡车上的最大单元数

__Description:__

You are assigned to put some amount of boxes onto __one truck__. You are given a 2D array `boxTypes`, where `boxTypes[i] = [numberOfBoxesi, numberOfUnitsPerBoxi]`:

- `numberOfBoxesi` is the number of boxes of type `i`.
- `numberOfUnitsPerBoxi` is the number of units in each box of the type `i`.

You are also given an integer `truckSize`, which is the __maximum__ number of __boxes__ that can be put on the truck. You can choose any boxes to put on the truck as long as the number of boxes does not exceed `truckSize`.

Return the maximum total number of units that can be put on the truck.

__Example:__

Example 1:

```text
Input: boxTypes = [[1,3],[2,2],[3,1]], truckSize = 4
Output: 8
Explanation: There are:
- 1 box of the first type that contains 3 units.
- 2 boxes of the second type that contain 2 units each.
- 3 boxes of the third type that contain 1 unit each.
You can take all the boxes of the first and second types, and one box of the third type.
The total number of units will be = (1 * 3) + (2 * 2) + (1 * 1) = 8.
```

Example 2:

```text
Input: boxTypes = [[5,10],[2,5],[4,7],[3,9]], truckSize = 10
Output: 91
```

__Constraints:__

- `1 <= boxTypes.length <= 1000`
- `1 <= numberOfBoxesi, numberOfUnitsPerBoxi <= 1000`
- `1 <= truckSize <= 10 ^ 6`

__题目描述:__

请你将一些箱子装在 __一辆卡车__ 上。给你一个二维数组 `boxTypes` ，其中 `boxTypes[i] = [numberOfBoxesi, numberOfUnitsPerBoxi]` :

- `numberOfBoxesi` 是类型 `i` 的箱子的数量。
- `numberOfUnitsPerBoxi` 是类型 `i` 每个箱子可以装载的单元数量。

整数 `truckSize` 表示卡车上可以装载 __箱子__ 的 __最大数量__ 。只要箱子数量不超过 `truckSize` ，你就可以选择任意箱子装到卡车上。

返回卡车可以装载 单元 的 最大 总数。

__示例:__

示例 1：

```text
输入：boxTypes = [[1,3],[2,2],[3,1]], truckSize = 4
输出：8
解释：箱子的情况如下：
- 1 个第一类的箱子，里面含 3 个单元。
- 2 个第二类的箱子，每个里面含 2 个单元。
- 3 个第三类的箱子，每个里面含 1 个单元。
可以选择第一类和第二类的所有箱子，以及第三类的一个箱子。
单元总数 = (1 * 3) + (2 * 2) + (1 * 1) = 8
```

示例 2：

```text
输入：boxTypes = [[5,10],[2,5],[4,7],[3,9]], truckSize = 10
输出：91
```

__提示：__

- `1 <= boxTypes.length <= 1000`
- `1 <= numberOfBoxesi, numberOfUnitsPerBoxi <= 1000`
- `1 <= truckSize <= 10 ^ 6`

__思路:__

```text
贪心
按照最多单元数进行排序
先把单元数最多的箱子装车直到装满或者装完所有箱子
累计箱子数和单元数的乘积
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumUnits(vector<vector<int>>& boxTypes, int truckSize) 
    {
        sort(boxTypes.begin(), boxTypes.end(), [](const auto& a, const auto& b) { return a.back() > b.back(); });
        int result = 0, n = boxTypes.size();
        for (int i = 0; i < n and truckSize; i++) 
        {
            result += min(boxTypes[i].front(), truckSize) * boxTypes[i].back();
            truckSize = max(truckSize - boxTypes[i].front(), 0);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumUnits(int[][] boxTypes, int truckSize) {
        Arrays.sort(boxTypes, Comparator.comparingInt(o -> -o[1]));
        int result = 0, n = boxTypes.length;
        for (int i = 0; i < n && truckSize != 0; i++) {
            result += Math.min(boxTypes[i][0], truckSize) * boxTypes[i][1];
            truckSize = Math.max(truckSize - boxTypes[i][0], 0);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumUnits(self, boxTypes: List[List[int]], truckSize: int) -> int:
        boxTypes.sort(key=lambda x: -x[1])
        result, n = 0, len(boxTypes)
        for i in range(n):
            result += min(boxTypes[i][0], truckSize) * boxTypes[i][1];
            truckSize = max(truckSize - boxTypes[i][0], 0);
            if not truckSize:
                break
        return result
```
