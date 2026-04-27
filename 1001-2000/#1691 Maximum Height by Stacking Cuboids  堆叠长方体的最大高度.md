# 1691 Maximum Height by Stacking Cuboids  堆叠长方体的最大高度

__Description:__

Given `n` `cuboids` where the dimensions of the `i ^ th` cuboid is `cuboids[i] = [widthi, lengthi, heighti]` (__0-indexed__). Choose a __subset__ of `cuboids` and place them on each other.

You can place cuboid `i` on cuboid `j` if `widthi <= widthj` and `lengthi <= lengthj` and `heighti <= heightj`. You can rearrange any cuboid's dimensions by rotating it to put it on another cuboid.

Return _the __maximum height__ of the stacked_ `cuboids`.

__Example:__

Example 1:

![1691-1](https://assets.leetcode.com/uploads/2019/10/21/image.jpg)

```text
Input: cuboids = [[50,45,20],[95,37,53],[45,23,12]]
Output: 190
Explanation:
Cuboid 1 is placed on the bottom with the 53x37 side facing down with height 95.
Cuboid 0 is placed next with the 45x20 side facing down with height 50.
Cuboid 2 is placed next with the 23x12 side facing down with height 45.
The total height is 95 + 50 + 45 = 190.
```

Example 2:

```text
Input: cuboids = [[38,25,45],[76,35,3]]
Output: 76
Explanation:
You can't place any of the cuboids on the other.
We choose cuboid 1 and rotate it so that the 35x3 side is facing down and its height is 76.
```

Example 3:

```text
Input: cuboids = [[7,11,17],[7,17,11],[11,7,17],[11,17,7],[17,7,11],[17,11,7]]
Output: 102
Explanation:
After rearranging the cuboids, you can see that all cuboids have the same dimension.
You can place the 11x7 side down on all cuboids so their heights are 17.
The maximum height of stacked cuboids is 6 * 17 = 102.
```

__Constraints:__

- `n == cuboids.length`
- `1 <= n <= 100`
- `1 <= widthi, lengthi, heighti <= 100`

__题目描述:__

给你 `n` 个长方体 `cuboids` ，其中第 `i` 个长方体的长宽高表示为 `cuboids[i] = [widthi, lengthi, heighti]`（__下标从 0 开始__）。请你从 `cuboids` 选出一个 __子集__ ，并将它们堆叠起来。

如果 `widthi <= widthj` 且 `lengthi <= lengthj` 且 `heighti <= heightj` ，你就可以将长方体 `i` 堆叠在长方体 `j` 上。你可以通过旋转把长方体的长宽高重新排列，以将它放在另一个长方体上。

返回 __堆叠长方体__ `cuboids` 可以得到的 __最大高度__ 。

__示例:__

示例 1：

![1691-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/12/12/image.jpg)

```text
输入：cuboids = [[50,45,20],[95,37,53],[45,23,12]]
输出：190
解释：
第 1 个长方体放在底部，53x37 的一面朝下，高度为 95 。
第 0 个长方体放在中间，45x20 的一面朝下，高度为 50 。
第 2 个长方体放在上面，23x12 的一面朝下，高度为 45 。
总高度是 95 + 50 + 45 = 190 。
```

示例 2：

```text
输入：cuboids = [[38,25,45],[76,35,3]]
输出：76
解释：
无法将任何长方体放在另一个上面。
选择第 1 个长方体然后旋转它，使 35x3 的一面朝下，其高度为 76 。
```

示例 3：

```text
输入：cuboids = [[7,11,17],[7,17,11],[11,7,17],[11,17,7],[17,7,11],[17,11,7]]
输出：102
解释：
重新排列长方体后，可以看到所有长方体的尺寸都相同。
你可以把 11x7 的一面朝下，这样它们的高度就是 17 。
堆叠长方体的最大高度为 6 * 17 = 102 。
```

__提示：__

- `n == cuboids.length`
- `1 <= n <= 100`
- `1 <= widthi, lengthi, heighti <= 100`

__思路:__

```text
动态规划 ➕ 排序
对于合法的堆叠, 我们可以将长方体排序使得高为最长边, 这个堆叠仍然是合法的
然后将所有长方体按升序排列
设 dp[i] 表示以第 i 个长方体为底堆叠的最高高度, dp[i] = max(dp[j]) + height[i], 其中 j < i 且 width[j] <= width[i] and length[j] <= length[i] and height[j] <= height[i]
由于长方体已经按升序排列, 因此我们只需要枚举 j < i 且 length[j] <= length[i] and height[j] <= height[i] 的 j 即可
遍历长方体并记录最高高度即可
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution
{
public:
    int maxHeight(vector<vector<int>>& cuboids) 
    {
        int n = cuboids.size();
        for (int i = 0; i < n; i++) sort(cuboids[i].begin(), cuboids[i].end());
        sort(cuboids.begin(), cuboids.end());
        vector<int> dp(n);
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < i; j++) if(cuboids[j][1] <= cuboids[i][1] and cuboids[j].back() <= cuboids[i].back()) dp[i] = max(dp[j], dp[i]);
            dp[i] += cuboids[i][2];
        }
        return *max_element(dp.begin(), dp.end());
    }
};
```

__Java__:

```Java
class Solution {
    public int maxHeight(int[][] cuboids) {
        for (int[] c : cuboids) Arrays.sort(c);
        Arrays.sort(cuboids, Comparator.<int[]>comparingInt(arr -> arr[0]).thenComparingInt(arr -> arr[1]).thenComparingInt(arr -> arr[2]));
        int n = cuboids.length, dp[] = new int[n], result = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) if (cuboids[j][1] <= cuboids[i][1] && cuboids[j][2] <= cuboids[i][2]) dp[i] = Math.max(dp[i], dp[j]);
            dp[i] += cuboids[i][2];
            result = Math.max(result, dp[i]);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxHeight(self, cuboids: List[List[int]]) -> int:
        cuboids, dp = sorted([sorted(c) for c in cuboids]), [0] * (n := len(cuboids))
        for i in range(n):
            for j in range(i):
                if cuboids[j][1] <= cuboids[i][1] and cuboids[j][2] <= cuboids[i][2]:
                    dp[i] = max(dp[i], dp[j])
            dp[i] += cuboids[i][2]
        return max(dp)
```
