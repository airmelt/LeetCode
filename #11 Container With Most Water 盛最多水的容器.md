# 11 Container With Most Water 盛最多水的容器

__Description__:
Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

__Note:__ You may not slant the container and n is at least 2.

![The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

__Example:__

Input: [1,8,6,2,5,4,8,3,7]
Output: 49

__题目描述__:
给定 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

__说明：__
你不能倾斜容器，且 n 的值至少为 2。

![图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

__示例 :__

输入: [1,8,6,2,5,4,8,3,7]
输出: 49

__思路__:

由于两边刚好对应数组两端, 考虑使用双指针, 由于两端向中间搜索的过程中, 两端的距离一定是减小的, 如果要增大乘积, 则必须要增大两端的长度, 所以要移动较短边
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maxArea(vector<int>& height) 
    {
        int result = min(height.front(), height.back()) * (height.size() - 1);
        for (int left = 0, right = height.size() - 1; left < right; ) result = max(result, (right - left) * (height[left] < height[right] ? height[left++] : height[right--]));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxArea(int[] height) {
        int left = 0, right = height.length - 1, result = 0;
        while (left < right) {
            result = Math.max(result, Math.min(height[left], height[right]) * (right - left));
            if (height[left] > height[right]) --right;
            else ++left;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        i, j, result = 0, len(height) - 1, 0
        while i < j:
            result = max(min(height[i], height[j]) * (j - i), result)
            if height[i] > height[j]:
                j -= 1
            else:
                i += 1
        return result
```
