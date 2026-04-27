# 1465 Maximum Area of a Piece of Cake After Horizontal and Vertical Cuts 切割后面积最大的蛋糕

__Description:__

You are given a rectangular cake of size  `h x w` and two arrays of integers  `horizontalCuts` and  `verticalCuts` where:

- `horizontalCuts[i]` is the distance from the top of the rectangular cake to the  `i ^ th` horizontal cut and similarly, and
- `verticalCuts[j]` is the distance from the left of the rectangular cake to the  `j ^ th` vertical cut.

Return _the maximum area of a piece of cake after you cut at each horizontal and vertical position provided in the arrays_  `horizontalCuts` _and_  `verticalCuts`. Since the answer can be a large number, return this __modulo__  `10 ^ 9 + 7`.

__Example:__

Example 1:

![1465-1](https://assets.leetcode.com/uploads/2020/05/14/leetcode_max_area_2.png)

```text
Input: h = 5, w = 4, horizontalCuts = [1,2,4], verticalCuts = [1,3]
Output: 4 
Explanation: The figure above represents the given rectangular cake. Red lines are the horizontal and vertical cuts. After you cut the cake, the green piece of cake has the maximum area.
```

Example 2:

![1465-2](https://assets.leetcode.com/uploads/2020/05/14/leetcode_max_area_3.png)

```text
Input: h = 5, w = 4, horizontalCuts = [3,1], verticalCuts = [1]
Output: 6
Explanation: The figure above represents the given rectangular cake. Red lines are the horizontal and vertical cuts. After you cut the cake, the green and yellow pieces of cake have the maximum area.
```

Example 3:

```text
Input: h = 5, w = 4, horizontalCuts = [3], verticalCuts = [3]
Output: 9
```

__Constraints:__

- `2 <= h, w <= 10 ^ 9`
- `1 <= horizontalCuts.length <= min(h - 1, 10 ^ 5)`
- `1 <= verticalCuts.length <= min(w - 1, 10 ^ 5)`
- `1 <= horizontalCuts[i] < h`
- `1 <= verticalCuts[i] < w`
- All the elements in  `horizontalCuts` are distinct.
- All the elements in  `verticalCuts` are distinct.

__题目描述:__

矩形蛋糕的高度为  `h` 且宽度为  `w`，给你两个整数数组  `horizontalCuts` 和  `verticalCuts`，其中：

- `horizontalCuts[i]` 是从矩形蛋糕顶部到第   `i` 个水平切口的距离
- `verticalCuts[j]` 是从矩形蛋糕的左侧到第  `j` 个竖直切口的距离

请你按数组 _`horizontalCuts`_ 和 _`verticalCuts`_ 中提供的水平和竖直位置切割后，请你找出 __面积最大__ 的那份蛋糕，并返回其 __面积__ 。由于答案可能是一个很大的数字，因此需要将结果 __对__  `10 ^ 9 + 7` __取余__ 后返回。

__示例:__

示例 1：

![1465-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/30/leetcode_max_area_2.png)

```text
输入：h = 5, w = 4, horizontalCuts = [1,2,4], verticalCuts = [1,3]
输出：4 
解释：上图所示的矩阵蛋糕中，红色线表示水平和竖直方向上的切口。切割蛋糕后，绿色的那份蛋糕面积最大。
```

示例 2：

![1465-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/30/leetcode_max_area_3.png)

```text
输入：h = 5, w = 4, horizontalCuts = [3,1], verticalCuts = [1]
输出：6
解释：上图所示的矩阵蛋糕中，红色线表示水平和竖直方向上的切口。切割蛋糕后，绿色和黄色的两份蛋糕面积最大。
```

示例 3：

```text
输入：h = 5, w = 4, horizontalCuts = [3], verticalCuts = [3]
输出：9
```

__提示：__

- `2 <= h, w <= 10 ^ 9`
- `1 <= horizontalCuts.length <= min(h - 1, 10 ^ 5)`
- `1 <= verticalCuts.length <= min(w - 1, 10 ^ 5)`
- `1 <= horizontalCuts[i] < h`
- `1 <= verticalCuts[i] < w`
- 题目数据保证  `horizontalCuts` 中的所有元素各不相同
- 题目数据保证  `verticalCuts` 中的所有元素各不相同

__思路:__

```text
贪心法
先对数组进行排序
然后选择相邻两个数的最大差值
并求最大差值的乘积即为最大巨星面积
时间复杂度为 O(NlgN), 空间复杂度为 O(lgN)
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    long helper(vector<int>& arr, int len) 
    {
        sort(arr.begin(), arr.end());
        long num = 0, pre = 0, n = arr.size();
        for (int i = 0; i < n; pre = arr[i++]) num = max(num, arr[i] - pre);
        return max(num, len - pre);
    }
public:
    int maxArea(int h, int w, vector<int>& horizontalCuts, vector<int>& verticalCuts) 
    {
        return (int)((helper(verticalCuts, w) * helper(horizontalCuts, h)) % (long)(1e9 + 7));
    }
};
```

__Java__:

```Java
class Solution {
    public int maxArea(int h, int w, int[] horizontalCuts, int[] verticalCuts) {
        return (int)((helper(verticalCuts, w) * helper(horizontalCuts, h)) % (1_000_000_007));
    }
    
    private long helper(int[] arr, int len) {
        Arrays.sort(arr);
        long num = 0, n = arr.length, pre = 0;
        for (int i = 0; i < n; pre = arr[i++]) num = Math.max(num, arr[i] - pre);
        return Math.max(num, len - pre);
    }
}
```

__Python__:

```Python
class Solution:
    def maxArea(self, h: int, w: int, horizontalCuts: List[int], verticalCuts: List[int]) -> int:
        return max(y - x for x, y in pairwise(sorted([0, *horizontalCuts, h]))) * max(y - x for x, y in pairwise(sorted([0, *verticalCuts, w]))) % (10 ** 9 + 7)
```
