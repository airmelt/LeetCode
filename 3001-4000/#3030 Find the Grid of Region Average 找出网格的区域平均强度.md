# 3030 Find the Grid of Region Average 找出网格的区域平均强度

__Description:__

You are given `m x n` grid `image` which represents a grayscale image, where `image[i][j]` represents a pixel with intensity in the range `[0..255]`. You are also given a __non-negative__ integer `threshold`.

Two pixels are adjacent if they share an edge.

A __region__ is a `3 x 3` subgrid where the __absolute difference__ in intensity between any two __adjacent__ pixels is __less than or equal to__ `threshold`.

All pixels in a region belong to that region, note that a pixel can belong to multiple regions.

You need to calculate a `m x n` grid `result`, where `result[i][j]` is the __average__ intensity of the regions to which `image[i][j]` belongs, __rounded down__ to the nearest integer. If `image[i][j]` belongs to multiple regions, `result[i][j]` is the __average__ of the __rounded-down average__ intensities of these regions, __rounded down__ to the nearest integer. If `image[i][j]` does __not__ belong to any region, `result[i][j]` is __equal to__ `image[i][j]`.

Return the grid `result`.

__Example:__

Example 1:

```text
Input: image = [[5,6,7,10],[8,9,10,10],[11,12,13,10]], threshold = 3

Output: [[9,9,9,9],[9,9,9,9],[9,9,9,9]]

Explanation:
```

![3030-1](https://assets.leetcode.com/uploads/2023/12/21/example0corrected.png)

There are two regions as illustrated above. The average intensity of the first region is 9, while the average intensity of the second region is 9.67 which is rounded down to 9. The average intensity of both of the regions is (9 + 9) / 2 = 9. As all the pixels belong to either region 1, region 2, or both of them, the intensity of every pixel in the result is 9.

Please note that the rounded-down values are used when calculating the average of multiple regions, hence the calculation is done using 9 as the average intensity of region 2, not 9.67.

Example 2:

```text
Input: image = [[10,20,30],[15,25,35],[20,30,40],[25,35,45]], threshold = 12

Output: [[25,25,25],[27,27,27],[27,27,27],[30,30,30]]

Explanation:
```

![3030-2](https://assets.leetcode.com/uploads/2023/12/21/example1corrected.png)

There are two regions as illustrated above. The average intensity of the first region is 25, while the average intensity of the second region is 30. The average intensity of both of the regions is (25 + 30) / 2 = 27.5 which is rounded down to 27.

All the pixels in row 0 of the image belong to region 1, hence all the pixels in row 0 in the result are 25. Similarly, all the pixels in row 3 in the result are 30. The pixels in rows 1 and 2 of the image belong to region 1 and region 2, hence their assigned value is 27 in the result.

Example 3:

```text
Input: image = [[5,6,7],[8,9,10],[11,12,13]], threshold = 1

Output: [[5,6,7],[8,9,10],[11,12,13]]

Explanation:
```

There is only one `3 x 3` subgrid, while it does not have the condition on difference of adjacent pixels, for example, the difference between `image[0][0]` and `image[1][0]` is `|5 - 8| = 3 > threshold = 1`. None of them belong to any valid regions, so the `result` should be the same as `image`.

__Constraints:__

- `3 <= n, m <= 500`
- `0 <= image[i][j] <= 255`
- `0 <= threshold <= 255`

__题目描述:__

给你一个下标从 __0__ 开始、大小为 `m x n` 的网格 `image` ，表示一个灰度图像，其中 `image[i][j]` 表示在范围 `[0..255]` 内的某个像素强度。另给你一个 __非负__ 整数 `threshold` 。

如果 `image[a][b]` 和 `image[c][d]` 满足 `|a - c| + |b - d| == 1` ，则称这两个像素是 __相邻像素__ 。

__区域__ 是一个 `3 x 3` 的子网格，且满足区域中任意两个 __相邻__ 像素之间，像素强度的 __绝对差__ __小于或等于__ `threshold` 。

区域 内的所有像素都认为属于该区域，而一个像素 可以 属于 多个 区域。

你需要计算一个下标从 __0__ 开始、大小为 `m x n` 的网格 `result` ，其中 `result[i][j]` 是 `image[i][j]` 所属区域的 __平均__ 强度，__向下取整__ 到最接近的整数。如果 `image[i][j]` 属于多个区域， `result[i][j]` 是这些区域的 __“取整后的平均强度”__ 的 __平均值__，也 __向下取整__ 到最接近的整数。如果 `image[i][j]` 不属于任何区域，则 `result[i][j]` __等于__ `image[i][j]` 。

返回网格 `result` 。

__示例:__

示例 1：

![3030-3](https://assets.leetcode.com/uploads/2023/12/21/example0corrected.png)

```text
输入：image = [[5,6,7,10],[8,9,10,10],[11,12,13,10]], threshold = 3
输出：[[9,9,9,9],[9,9,9,9],[9,9,9,9]]
解释：图像中存在两个区域，如图片中的阴影区域所示。第一个区域的平均强度为 9 ，而第二个区域的平均强度为 9.67 ，向下取整为 9 。两个区域的平均强度为 (9 + 9) / 2 = 9 。由于所有像素都属于区域 1 、区域 2 或两者，因此 result 中每个像素的强度都为 9 。
注意，在计算多个区域的平均值时使用了向下取整的值，因此使用区域 2 的平均强度 9 来进行计算，而不是 9.67 。
```

示例 2：

![3030-4](https://assets.leetcode.com/uploads/2023/12/21/example1corrected.png)

```text
输入：image = [[10,20,30],[15,25,35],[20,30,40],[25,35,45]], threshold = 12
输出：[[25,25,25],[27,27,27],[27,27,27],[30,30,30]]
解释：图像中存在两个区域，如图片中的阴影区域所示。第一个区域的平均强度为 25 ，而第二个区域的平均强度为 30 。两个区域的平均强度为 (25 + 30) / 2 = 27.5 ，向下取整为 27 。图像中第 0 行的所有像素属于区域 1 ，因此 result 中第 0 行的所有像素为 25 。同理，result 中第 3 行的所有像素为 30 。图像中第 1 行和第 2 行的像素属于区域 1 和区域 2 ，因此它们在 result 中的值为 27 。
```

示例 3：

```text
输入：image = [[5,6,7],[8,9,10],[11,12,13]], threshold = 1
输出：[[5,6,7],[8,9,10],[11,12,13]]
解释：图像中不存在任何区域，因此对于所有像素，result[i][j] == image[i][j] 。
```

__提示：__

- `3 <= n, m <= 500`
- `0 <= image[i][j] <= 255`
- `0 <= threshold <= 255`

__思路:__

```text
模拟
以每个 3 X 3 的矩阵为界
先检查每对相邻坐标是否满足阈值
不满足直接跳过该区域
如果满足累加当前值到结果中并记下区域的数量
最后计算出平均值并除以当前元素的区域数并取整
时间复杂度为 O(MN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> resultGrid(vector<vector<int>>& image, int threshold) 
    {
        int m = image.size(), n = image.front().size(), avg = 0, flag = 1;
        vector<vector<int>> result(m, vector<int>(n)), c(m, vector<int>(n));
        for (int i = 2; i < m; i++) 
        {
            for (int j = 2; j < n; j++, avg = 0, flag = 1) 
            {
                for (int k = i - 2; flag and k <= i; k++) if (abs(image[k][j - 2] - image[k][j - 1]) > threshold or abs(image[k][j - 1] - image[k][j]) > threshold) flag = 0;
                for (int k = j - 2; flag and k <= j; k++) if (abs(image[i - 2][k] - image[i - 1][k]) > threshold or abs(image[i - 1][k] - image[i][k]) > threshold) flag = 0;
                for (int x = i - 2; flag and x <= i; x++) for (int y = j - 2; y <= j; y++) avg += image[x][y];
                for (int x = i - 2; flag and x <= i; x++) for (int y = j - 2; y <= j; y++) result[x][y] += avg / 9;
                for (int x = i - 2; flag and x <= i; x++) for (int y = j - 2; y <= j; y++) ++c[x][y];
            }
        }
        for (int x = 0; x < m; x++) for (int y = 0; y < n; y++) result[x][y] = c[x][y] ? result[x][y] / c[x][y] : image[x][y];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] resultGrid(int[][] image, int threshold) {
        int m = image.length, n = image[0].length, result[][] = new int[m][n], c[][] = new int[m][n], avg = 0, flag = 1;
        for (int i = 2; i < m; i++) {
            for (int j = 2; j < n; j++, avg = 0, flag = 1) {
                for (int k = i - 2; flag > 0 && k <= i; k++) if (Math.abs(image[k][j - 2] - image[k][j - 1]) > threshold || Math.abs(image[k][j - 1] - image[k][j]) > threshold) flag = 0;
                for (int k = j - 2; flag > 0 && k <= j; k++) if (Math.abs(image[i - 2][k] - image[i - 1][k]) > threshold || Math.abs(image[i - 1][k] - image[i][k]) > threshold) flag = 0;
                for (int x = i - 2; flag > 0 && x <= i; x++) for (int y = j - 2; y <= j; y++) avg += image[x][y];
                for (int x = i - 2; flag > 0 && x <= i; x++) for (int y = j - 2; y <= j; y++) result[x][y] += avg / 9;
                for (int x = i - 2; flag > 0 && x <= i; x++) for (int y = j - 2; y <= j; y++) ++c[x][y];
            }
        }
        for (int x = 0; x < m; x++) for (int y = 0; y < n; y++) result[x][y] = c[x][y] > 0 ? result[x][y] / c[x][y] : image[x][y];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def resultGrid(self, image: List[List[int]], threshold: int) -> List[List[int]]:
        m, n = len(image), len(image[0])
        result, c = [[0] * n for _ in range(m)], [[0] * n for _ in range(m)]
        for i in range(2, m):
            for j in range(2, n):
                flag = True
                for row in image[i - 2:i + 1]:
                    if abs(row[j - 2] - row[j - 1]) > threshold or abs(row[j - 1] - row[j]) > threshold:
                        flag = False
                        break
                if not flag:
                    continue
                for y in range(j - 2, j + 1):
                    if abs(image[i - 2][y] - image[i - 1][y]) > threshold or abs(image[i - 1][y] - image[i][y]) > threshold:
                        flag = False
                        break
                if not flag:
                    continue
                avg = sum(image[x][y] for x in range(i - 2, i + 1) for y in range(j - 2, j + 1)) // 9
                for x in range(i - 2, i + 1):
                    for y in range(j - 2, j + 1):
                        result[x][y] += avg
                        c[x][y] += 1
        for i, row in enumerate(c):
            for j, v in enumerate(row):
                result[i][j] = (result[i][j] // v) if v else image[i][j]
        return result
```
