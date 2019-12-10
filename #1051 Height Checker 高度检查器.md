__Description__:
Students are asked to stand in non-decreasing order of heights for an annual photo.

Return the minimum number of students not standing in the right positions.  (This is the number of students that must move in order for all students to be standing in non-decreasing order of height.)

__Example:__
Example 1:

Input: [1,1,4,2,1,3]
Output: 3
Explanation: 
Students with heights 4, 3 and the last 1 are not standing in the right positions.
 
__Note:__

1 <= heights.length <= 100
1 <= heights[i] <= 100

__题目描述__:
学校在拍年度纪念照时，一般要求学生按照 非递减 的高度顺序排列。

请你返回至少有多少个学生没有站在正确位置数量。该人数指的是：能让所有学生以 非递减 高度排列的必要移动人数。

__示例 :__

输入：[1,1,4,2,1,3]
输出：3
解释：
高度为 4、3 和最后一个 1 的学生，没有站在正确的位置。
 
__提示：__

1 <= heights.length <= 100
1 <= heights[i] <= 100

__思路__:
1. 比较排序和原数组的位置不同的个数
时间复杂度O(nlgn), 空间复杂度O(n)
2. 利用桶排序, 将高度放入不同的桶中, 遍历桶如果桶中的元素跟原高度数组的元素不同, 说明没有在递增序列上, 结果 + 1
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int heightChecker(vector<int>& heights) 
    {
        int count[101]{0};
        for (auto height : heights) count[height]++;
        int result = 0;
        for (int i = 1, j = 0; i < 101; ++i) while (count[i]-- > 0) if (heights[j++] != i) ++result;
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int heightChecker(int[] heights) {
        int[] count = new int[101];
        for (int height : heights) count[height]++;
        int result = 0;
        for (int i = 1, j = 0; i < count.length; i++) while (count[i]-- > 0) if (heights[j++] != i) result++;
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def heightChecker(self, heights: List[int]) -> int:
        return sum(h1 != h2 for h1, h2 in zip(heights, sorted(heights)))
```