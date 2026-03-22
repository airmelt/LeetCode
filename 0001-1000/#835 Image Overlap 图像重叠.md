# 835 Image Overlap 图像重叠

__Description__:
You are given two images, img1 and img2, represented as binary, square matrices of size n x n. A binary matrix has only 0s and 1s as values.

We translate one image however we choose by sliding all the 1 bits left, right, up, and/or down any number of units. We then place it on top of the other image. We can then calculate the overlap by counting the number of positions that have a 1 in both images.

Note also that a translation does not include any kind of rotation. Any 1 bits that are translated outside of the matrix borders are erased.

Return the largest possible overlap.

__Example:__

Example 1:

![overlap1](https://assets.leetcode.com/uploads/2020/09/09/overlap1.jpg)

Input: img1 = [[1,1,0],[0,1,0],[0,1,0]], img2 = [[0,0,0],[0,1,1],[0,0,1]]
Output: 3
Explanation: We translate img1 to right by 1 unit and down by 1 unit.

![overlap_step1](https://assets.leetcode.com/uploads/2020/09/09/overlap_step1.jpg)

The number of positions that have a 1 in both images is 3 (shown in red).

![overlap_step2](https://assets.leetcode.com/uploads/2020/09/09/overlap_step2.jpg)

Example 2:

Input: img1 = [[1]], img2 = [[1]]
Output: 1

Example 3:

Input: img1 = [[0]], img2 = [[0]]
Output: 0

__Constraints:__

n == img1.length == img1[i].length
n == img2.length == img2[i].length
1 <= n <= 30
img1[i][j] is either 0 or 1.
img2[i][j] is either 0 or 1.

__题目描述__:
给你两个图像 img1 和 img2 ，两个图像的大小都是 n x n ，用大小相同的二维正方形矩阵表示。（并且为二进制矩阵，只包含若干 0 和若干 1 ）

转换其中一个图像，向左，右，上，或下滑动任何数量的单位，并把它放在另一个图像的上面。之后，该转换的 重叠 是指两个图像都具有 1 的位置的数目。

（请注意，转换 不包括 向任何方向旋转。）

最大可能的重叠是多少？

__示例 :__

示例 1：

![重叠 1](https://assets.leetcode.com/uploads/2020/09/09/overlap1.jpg)

输入：img1 = [[1,1,0],[0,1,0],[0,1,0]], img2 = [[0,0,0],[0,1,1],[0,0,1]]
输出：3
解释：将 img1 向右移动 1 个单位，再向下移动 1 个单位。

![步骤 1](https://assets.leetcode.com/uploads/2020/09/09/overlap_step1.jpg)

两个图像都具有 1 的位置的数目是 3（用红色标识）。

![步骤 2](https://assets.leetcode.com/uploads/2020/09/09/overlap_step2.jpg)

示例 2：

输入：img1 = [[1]], img2 = [[1]]
输出：1

示例 3：

输入：img1 = [[0]], img2 = [[0]]
输出：0

__提示:__

n == img1.length
n == img1[i].length
n == img2.length
n == img2[i].length
1 <= n <= 30
img1[i][j] 为 0 或 1
img2[i][j] 为 0 或 1

__思路__:

哈希表
记录下每个 1 的位置, 因为 n 不超过 30, 可以转化为 1 维坐标记录
然后将两个坐标的偏移量记录在哈希表中
返回最大的偏移量的数量
时间复杂度为 O(n ^ 4), 空间复杂度为 O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int largestOverlap(vector<vector<int>>& img1, vector<vector<int>>& img2) 
    {
        int n = img1.size(), result = 0;
        vector<int> grid1, grid2;
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                if (img1[i][j]) grid1.emplace_back(i * 100 + j);
                if (img2[i][j]) grid2.emplace_back(i * 100 + j);
            }
        }
        unordered_map<int, int> count;
        for (const auto& i : grid1) for (const auto& j : grid2) ++count[i - j];
        for (const auto& [k, v] : count) result = max(result, v);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int largestOverlap(int[][] img1, int[][] img2) {
        List<Integer> grid1 = new ArrayList<>(), grid2 = new ArrayList<>();
        int n = img1.length, result = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (img1[i][j] == 1) grid1.add(i * 100 + j);
                if (img2[i][j] == 1) grid2.add(i * 100 + j);
            }
        }
        Map<Integer, Integer> map = new HashMap<>();
        for (int i : grid1) for (int j : grid2) map.put(i - j, map.getOrDefault(i - j, 0) + 1);
        for (int v : map.values()) result = Math.max(result, v);
        return result;
    }
}
```

__Python__:

```Python
import numpy as np
class Solution:
    def largestOverlap(self, img1: List[List[int]], img2: List[List[int]]) -> int:
        N = len(img1)
        N2 = 1 << (N.bit_length() + 1)
        conv = np.fft.ifft2(np.fft.fft2(np.array(img1), (N2, N2)) * np.fft.fft2(np.array(img2)[::-1, ::-1], (N2, N2)))
        return int(np.round(np.max(conv)))
```
