__Description__:
Given n points on a 2D plane, find the maximum number of points that lie on the same straight line.

Example 1:

Input: [[1,1],[2,2],[3,3]]
Output: 3
Explanation:
```
^
|
|        o
|     o
|  o  
+------------->
0  1  2  3  4
```

Example 2:

Input: [[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]
Output: 4
Explanation:
```
^
|
|  o
|     o        o
|        o
|  o        o
+------------------->
0  1  2  3  4  5  6
```

__NOTE:__
input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

__题目描述__:
在 O(n log n) 时间复杂度和常数级空间复杂度下，对链表进行排序。

__示例 :__
示例 1:

输入: 4->2->1->3
输出: 1->2->3->4
示例 2:

输入: -1->5->3->4->0
输出: -1->0->3->4->5

__思路__:
参考[LeetCode #1232 Check If It Is a Straight Line 缀点成线](https://www.jianshu.com/p/565e452159f5)
对每一个点, 在后面的列表中找相同的点和在同一条直线上的点
为了防止溢出, 可以用 long存储乘积
时间复杂度O(n ^ 2), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int maxPoints(vector<vector<int>>& points) 
    {
        if (points.size() < 3) return points.size();
        int result = 0, len = points.size();
        for (int i = 0; i < len; i++)
        {
            int same = 1;
            for (int j = i + 1; j < len; j++)
            {
                int count = 0;
                if (points[i][0] == points[j][0] and points[i][1] == points[j][1]) ++same;
                else
                {
                    ++count;
                    long delta_x = (long)(points[i][0] - points[j][0]), delta_y = (long)(points[i][1] - points[j][1]);
                    for (int k = j + 1; k < len; k++) if (delta_x * (points[i][1] - points[k][1]) == delta_y * (points[i][0] - points[k][0])) ++count;
                }
                result = max(result, same + count);
            }
            if (result > (len >> 1)) return result;
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int maxPoints(int[][] points) {
        if (points.length < 3) return points.length;
        int result = 0, len = points.length;
        for (int i = 0; i < len; i++)
        {
            int same = 1;
            for (int j = i + 1; j < len; j++)
            {
                int count = 0;
                if (points[i][0] == points[j][0] && points[i][1] == points[j][1]) ++same;
                else
                {
                    ++count;
                    long deltaX = (long)(points[i][0] - points[j][0]), deltaY = (long)(points[i][1] - points[j][1]);
                    for (int k = j + 1; k < len; k++) if (deltaX * (points[i][1] - points[k][1]) == deltaY * (points[i][0] - points[k][0])) ++count;
                }
                result = Math.max(result, same + count);
            }
            if (result > (len >>> 1)) return result;
        }
        return result;
    }
}
```

__Python__:
```Python
from decimal import Decimal
class Solution:
    def maxPoints(self, points: List[List[int]]) -> int:
        return max(sum(1 for j in points if j == i) + Counter([float('Inf') if i[1] - j[1] == 0 else Decimal(i[0] - j[0]) / Decimal(i[1] - j[1]) for j in points if j != i]).most_common(1)[0][1] if sum(1 for j in points if j == i) != len(points) else sum(1 for j in points if j == i) for i in points) if len(points) > 2 else len(points)
```