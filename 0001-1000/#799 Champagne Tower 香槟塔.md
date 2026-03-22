# 799 Champagne Tower 香槟塔

__Description__:
We stack glasses in a pyramid, where the first row has 1 glass, the second row has 2 glasses, and so on until the 100th row.  Each glass holds one cup of champagne.

Then, some champagne is poured into the first glass at the top.  When the topmost glass is full, any excess liquid poured will fall equally to the glass immediately to the left and right of it.  When those glasses become full, any excess champagne will fall equally to the left and right of those glasses, and so on.  (A glass at the bottom row has its excess champagne fall on the floor.)

For example, after one cup of champagne is poured, the top most glass is full.  After two cups of champagne are poured, the two glasses on the second row are half full.  After three cups of champagne are poured, those two cups become full - there are 3 full glasses total now.  After four cups of champagne are poured, the third row has the middle glass half full, and the two outside glasses are a quarter full, as pictured below.

![Champagne Tower](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/03/09/tower.png)

Now after pouring some non-negative integer cups of champagne, return how full the jth glass in the ith row is (both i and j are 0-indexed.)

__Example:__

Example 1:

Input: poured = 1, query_row = 1, query_glass = 1
Output: 0.00000
Explanation: We poured 1 cup of champange to the top glass of the tower (which is indexed as (0, 0)). There will be no excess liquid so all the glasses under the top glass will remain empty.
Example 2:

Input: poured = 2, query_row = 1, query_glass = 1
Output: 0.50000
Explanation: We poured 2 cups of champange to the top glass of the tower (which is indexed as (0, 0)). There is one cup of excess liquid. The glass indexed as (1, 0) and the glass indexed as (1, 1) will share the excess liquid equally, and each will get half cup of champange.
Example 3:

Input: poured = 100000009, query_row = 33, query_glass = 17
Output: 1.00000

__Constraints:__

0 <= poured <= 10^9
0 <= query_glass <= query_row < 100

__题目描述__:
我们把玻璃杯摆成金字塔的形状，其中第一层有1个玻璃杯，第二层有2个，依次类推到第100层，每个玻璃杯(250ml)将盛有香槟。

从顶层的第一个玻璃杯开始倾倒一些香槟，当顶层的杯子满了，任何溢出的香槟都会立刻等流量的流向左右两侧的玻璃杯。当左右两边的杯子也满了，就会等流量的流向它们左右两边的杯子，依次类推。（当最底层的玻璃杯满了，香槟会流到地板上）

例如，在倾倒一杯香槟后，最顶层的玻璃杯满了。倾倒了两杯香槟后，第二层的两个玻璃杯各自盛放一半的香槟。在倒三杯香槟后，第二层的香槟满了 - 此时总共有三个满的玻璃杯。在倒第四杯后，第三层中间的玻璃杯盛放了一半的香槟，他两边的玻璃杯各自盛放了四分之一的香槟，如下图所示。

![Champagne Tower](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/03/09/tower.png)

现在当倾倒了非负整数杯香槟后，返回第 i 行 j 个玻璃杯所盛放的香槟占玻璃杯容积的比例（i 和 j都从0开始）。

__示例 :__

示例 1:
输入: poured(倾倒香槟总杯数) = 1, query_glass(杯子的位置数) = 1, query_row(行数) = 1
输出: 0.0
解释: 我们在顶层（下标是（0，0））倒了一杯香槟后，没有溢出，因此所有在顶层以下的玻璃杯都是空的。

示例 2:
输入: poured(倾倒香槟总杯数) = 2, query_glass(杯子的位置数) = 1, query_row(行数) = 1
输出: 0.5
解释: 我们在顶层（下标是（0，0）倒了两杯香槟后，有一杯量的香槟将从顶层溢出，位于（1，0）的玻璃杯和（1，1）的玻璃杯平分了这一杯香槟，所以每个玻璃杯有一半的香槟。

__注意:__

poured 的范围[0, 10 ^ 9]。
query_glass 和query_row 的范围 [0, 99]。

__思路__:

动态规划
设 dp[i][j] 表示第 i 行第 j 杯的容量
令 dp[0][0] = poured, 即把所有酒都倒入最上面的酒杯中
dp[i + 1][j] = (max(dp[i][j] - 1, 0) + max(dp[i][j - 1] - 1, 0)) / 2
返回 min(1, dp[query_row][query_glass]) 即可, 注意不能溢出
可以看到 dp 只取决于前一行, 所以可以用滚动数组优化空间复杂度至 O(n)
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    double champagneTower(int poured, int query_row, int query_glass) 
    {
        vector<double> dp(query_row + 1);
        dp.front() = poured;
        for (int i = 1; i <= query_row; i++)
        {
            for (int j = i; j > 0; j--) dp[j] = (max(dp[j] - 1, 0.0) + max(dp[j - 1] - 1, 0.0)) / 2;
            dp.front() = max(dp.front() - 1, 0.0) / 2;
        }
        return min(1.0, dp[query_glass]);
    }
};
```

__Java__:

```Java
class Solution {
    public double champagneTower(int poured, int query_row, int query_glass) {
        double[][] dp = new double[query_row + 1][query_row + 1];
        dp[0][0] = poured;
        for (int i = 1; i <= query_row; ++i) {
            for (int j = 0; j <= i; ++j) {
                if (j > 0) dp[i][j] += Math.max((dp[i - 1][j - 1] - 1) / 2, 0);
                if (j < i) dp[i][j] += Math.max((dp[i - 1][j] - 1) / 2, 0);
            }
        }
        return Math.min(dp[query_row][query_glass], 1);
    }
}
```

__Python__:

```Python
class Solution:
    def champagneTower(self, poured: int, query_row: int, query_glass: int) -> float:
        dp = [poured] + [0.0] * query_row
        for i in range(1, query_row + 1):
            for j in range(i, 0, -1):
                dp[j] = (max(dp[j] - 1, 0.0) + max(dp[j - 1] - 1, 0.0)) / 2.0
            dp[0] = max(dp[0] - 1.0, 0.0) / 2.0
        return min(1.0, dp[query_glass])
```
