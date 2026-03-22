# 593 Valid Square 有效的正方形

__Description__:
Given the coordinates of four points in 2D space p1, p2, p3 and p4, return true if the four points construct a square.

The coordinate of a point pi is represented as [xi, yi]. The input is not given in any order.

A valid square has four equal sides with positive length and four equal angles (90-degree angles).

__Example:__

Example 1:

Input: p1 = [0,0], p2 = [1,1], p3 = [1,0], p4 = [0,1]
Output: true

Example 2:

Input: p1 = [0,0], p2 = [1,1], p3 = [1,0], p4 = [0,12]
Output: false

Example 3:

Input: p1 = [1,0], p2 = [-1,0], p3 = [0,1], p4 = [0,-1]
Output: true

__Constraints:__

p1.length == p2.length == p3.length == p4.length == 2
-10^4 <= xi, yi <= 10^4

__题目描述__:
给定二维空间中四点的坐标，返回四点是否可以构造一个正方形。

一个点的坐标（x，y）由一个有两个整数的整数数组表示。

__示例 :__

输入: p1 = [0,0], p2 = [1,1], p3 = [1,0], p4 = [0,1]
输出: True

__注意:__

所有输入整数都在 [-10000，10000] 范围内。
一个有效的正方形有四个等长的正长和四个等角（90度角）。
输入点没有顺序。

__思路__:

正方形的四条边都相等且对角线相等
注意正方形的边长不能为 0
时间复杂度 O(1), 空间复杂度 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool validSquare(vector<int>& p1, vector<int>& p2, vector<int>& p3, vector<int>& p4) 
    {
        unordered_set<int> s;
        s.insert(distance(p1, p2));
        s.insert(distance(p1, p3));
        s.insert(distance(p1, p4));
        s.insert(distance(p3, p2));
        s.insert(distance(p4, p2));
        s.insert(distance(p3, p4));
        return s.size() == 2 and !s.count(0);
    }
private:
    int distance(vector<int>& a, vector<int>& b) 
    {
        return (a[0] - b[0]) * (a[0] - b[0]) + (a[1] - b[1]) * (a[1] - b[1]);
    }
};
```

__Java__:

```Java
class Solution {
    public boolean validSquare(int[] p1, int[] p2, int[] p3, int[] p4) {
        Set<Integer> set = new HashSet<>();
        set.add(distance(p1, p2));
        set.add(distance(p1, p3));
        set.add(distance(p1, p4));
        set.add(distance(p3, p2));
        set.add(distance(p4, p2));
        set.add(distance(p3, p4));
        return set.size() == 2 && !set.contains(0);
    }
    
    private int distance(int[] a, int[] b) {
        return (a[0] - b[0]) * (a[0] - b[0]) + (a[1] - b[1]) * (a[1] - b[1]);
    }
}
```

__Python__:

```Python
class Solution:
    def validSquare(self, p1: List[int], p2: List[int], p3: List[int], p4: List[int]) -> bool:
        return 0 not in (s := set((x1 - x2) ** 2 + (y1 - y2) ** 2 for (x1, y1), (x2, y2) in combinations([p1, p2, p3, p4], 2))) and len(s) == 2
```
