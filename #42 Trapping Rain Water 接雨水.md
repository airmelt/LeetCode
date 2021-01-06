# 42 Trapping Rain Water 接雨水

__Description__:
Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

![The above elevation map is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.](https://upload-images.jianshu.io/upload_images/16639143-e2c4da8efa55c3ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

__Example:__

Input: [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6

__题目描述__:
给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

![
上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。](https://upload-images.jianshu.io/upload_images/16639143-e2c4da8efa55c3ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

__示例 :__

输入: [0,1,0,2,1,0,1,3,2,1,2,1]
输出: 6

__思路__:

使用双指针, 分别从两端遍历
记录下两端遍历时经过的最高的柱子的高度
每次取两端指针较矮的柱子, 更新最高的柱子的高度或者更新结果
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int trap(vector<int>& height) 
    {
        int result = 0, left = 0, right = height.size() - 1, left_max = 0, right_max = 0;
        while (left < right) 
        {
            if (height[left] < height[right]) 
            {
                if (height[left] >= left_max) left_max = height[left];
                else result += left_max - height[left];
                ++left;
            } 
            else 
            {
                if (height[right] >= right_max) right_max = height[right];
                else result += right_max - height[right];
                --right;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int trap(int[] height) {
        int result = 0, left = 0, right = height.length - 1, leftMax = 0, rightMax = 0;
        while (left < right) {
            if (height[left] < height[right]) {
                if (height[left] >= leftMax) leftMax = height[left];
                else result += leftMax - height[left];
                ++left;
            } else {
                if (height[right] >= rightMax) rightMax = height[right];
                else result += rightMax - height[right];
                --right;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def trap(self, height: List[int]) -> int:
        result, left, right, left_max, right_max = 0, 0, len(height) - 1, 0, 0
        while left < right:
            if height[left] < height[right]:
                if height[left] >= left_max:
                    left_max = height[left]
                else:
                    result += left_max - height[left]
                left += 1
            else:
                if height[right] >= right_max:
                    right_max = height[right]
                else:
                    result += right_max - height[right]
                right -= 1
        return result
```
