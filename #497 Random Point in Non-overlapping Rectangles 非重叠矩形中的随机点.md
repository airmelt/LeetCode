# 497 Random Point in Non-overlapping Rectangles 非重叠矩形中的随机点

__Description__:
Given a list of non-overlapping axis-aligned rectangles rects, write a function pick which randomly and uniformily picks an integer point in the space covered by the rectangles.

__Note:__

An integer point is a point that has integer coordinates.
A point on the perimeter of a rectangle is included in the space covered by the rectangles.
ith rectangle = rects[i] = [x1,y1,x2,y2], where [x1, y1] are the integer coordinates of the bottom-left corner, and [x2, y2] are the integer coordinates of the top-right corner.
length and width of each rectangle does not exceed 2000.
1 <= rects.length <= 100
pick return a point as an array of integer coordinates [p_x, p_y]
pick is called at most 10000 times.

__Example:__

Example 1:

Input:
["Solution","pick","pick","pick"]
[[[[1,1,5,5]]],[],[],[]]
Output:
[null,[4,1],[4,1],[3,3]]

Example 2:

Input:
["Solution","pick","pick","pick","pick","pick"]
[[[[-2,-2,-1,-1],[1,0,3,0]]],[],[],[],[],[]]
Output:
[null,[-1,-2],[2,0],[-2,-1],[3,0],[-2,-2]]

__Explanation of Input Syntax:__

The input is two lists: the subroutines called and their arguments. Solution's constructor has one argument, the array of rectangles rects. pick has no arguments. Arguments are always wrapped with a list, even if there aren't any.

__题目描述__:
给定一个非重叠轴对齐矩形的列表 rects，写一个函数 pick 随机均匀地选取矩形覆盖的空间中的整数点。

__提示:__

整数点是具有整数坐标的点。
矩形周边上的点包含在矩形覆盖的空间中。
第 i 个矩形 rects [i] = [x1，y1，x2，y2]，其中 [x1，y1] 是左下角的整数坐标，[x2，y2] 是右上角的整数坐标。
每个矩形的长度和宽度不超过 2000。
1 <= rects.length <= 100
pick 以整数坐标数组 [p_x, p_y] 的形式返回一个点。
pick 最多被调用10000次。

__示例 :__

示例 1：

输入:
["Solution","pick","pick","pick"]
[[[[1,1,5,5]]],[],[],[]]
输出:
[null,[4,1],[4,1],[3,3]]

示例 2：

输入:
["Solution","pick","pick","pick","pick","pick"]
[[[[-2,-2,-1,-1],[1,0,3,0]]],[],[],[],[],[]]
输出:
[null,[-1,-2],[2,0],[-2,-1],[3,0],[-2,-2]]

__输入语法的说明：__

输入是两个列表：调用的子例程及其参数。Solution 的构造函数有一个参数，即矩形数组 rects。pick 没有参数。参数总是用列表包装的，即使没有也是如此。

__思路__:

求每个矩阵的点数及点数的前缀和
然后用二分法求出矩阵的下标即可
预处理时间复杂度O(n), pick()时间复杂度O(lgn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    int area;
    map<int, int> m;
    vector<vector<int>> rects;
public:
    Solution(vector<vector<int>>& rects) 
    {
        this -> rects = rects;
        int sum = 0;
        for (int i = 0; i < rects.size(); i++)
        {
            int cur = (rects[i][2] - rects[i][0] + 1) * (rects[i][3] - rects[i][1] + 1);
            sum += cur;
            m[sum] = i;
        }
        area = sum;
    }
    
    vector<int> pick() 
    {
        random_device rd;
        default_random_engine e(rd());
        uniform_int_distribution dis(0, area);
        int index = m.upper_bound(dis(e)) -> second;
        uniform_int_distribution x(rects[index][0], rects[index][2]), y(rects[index][1], rects[index][3]);
        return { x(e), y(e) };
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(rects);
 * vector<int> param_1 = obj->pick();
 */
```

__Java__:

```Java
class Solution {
    private int[][] rects;
    private int len;
    private int pre[];
    private Random r = new Random();
    
    public Solution(int[][] rects) {
        pre = new int[rects.length + 1];
        for (int i = 0; i < rects.length; i++) pre[i + 1] += pre[i] + (rects[i][2]-rects[i][0] + 1) * (rects[i][3] - rects[i][1] + 1);
        this.rects = rects;
        len = pre[pre.length - 1];
    }
    
    public int[] pick() {
        int p = r.nextInt(len), left = 1, right = pre.length - 1;
        while (left < right) {
            int mid = left + ((right - left) >>> 1);
            if (p >= pre[mid]) left = mid + 1;
            else right = mid;
        }
        int index = p - pre[left - 1], col = rects[left - 1][3] - rects[left - 1][1] + 1;
        return new int[] { index / col + rects[left - 1][0], index % col + rects[left - 1][1] };
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(rects);
 * int[] param_1 = obj.pick();
 */
```

__Python__:

```Python
class Solution:

    def __init__(self, rects: List[List[int]]):
        self.rects = rects
        self.weight = []
        s = 0
        for rect in rects:
            area = (rect[2] - rect[0] + 1) * (rect[3] - rect[1] + 1)
            s += area
            self.weight.append(s)

    def pick(self) -> List[int]:
        index = bisect.bisect_left(self.weight, randint(1, self.weight[-1]))
        rect = self.rects[index]
        return [random.randint(rect[0], rect[2]), random.randint(rect[1], rect[3])]


# Your Solution object will be instantiated and called as such:
# obj = Solution(rects)
# param_1 = obj.pick()
```
