# 478 Generate Random Point in a Circle 在圆内随机生成点

__Description__:
Given the radius and x-y positions of the center of a circle, write a function randPoint which generates a uniform random point in the circle.

__Note:__

input and output values are in floating-point.
radius and x-y position of the center of the circle is passed into the class constructor.
a point on the circumference of the circle is considered to be in the circle.
randPoint returns a size 2 array containing x-position and y-position of the random point, in that order.

__Example:__

Example 1:

Input:
["Solution","randPoint","randPoint","randPoint"]
[[1,0,0],[],[],[]]
Output: [null,[-0.72939,-0.65505],[-0.78502,-0.28626],[-0.83119,-0.19803]]

Example 2:

Input:
["Solution","randPoint","randPoint","randPoint"]
[[10,5,-7.5],[],[],[]]
Output: [null,[11.52438,-8.33273],[2.46992,-16.21705],[11.13430,-12.42337]]
Explanation of Input Syntax:

The input is two lists: the subroutines called and their arguments. Solution's constructor has three arguments, the radius, x-position of the center, and y-position of the center of the circle. randPoint has no arguments. Arguments are always wrapped with a list, even if there aren't any.

__题目描述__:
给定圆的半径和圆心的 x、y 坐标，写一个在圆中产生均匀随机点的函数 randPoint 。

__注意:__

输入值和输出值都将是浮点数。
圆的半径和圆心的 x、y 坐标将作为参数传递给类的构造函数。
圆周上的点也认为是在圆中。
randPoint 返回一个包含随机点的x坐标和y坐标的大小为2的数组。

__示例 :__

示例 1：

输入:
["Solution","randPoint","randPoint","randPoint"]
[[1,0,0],[],[],[]]
输出: [null,[-0.72939,-0.65505],[-0.78502,-0.28626],[-0.83119,-0.19803]]

示例 2：

输入:
["Solution","randPoint","randPoint","randPoint"]
[[10,5,-7.5],[],[],[]]
输出: [null,[11.52438,-8.33273],[2.46992,-16.21705],[11.13430,-12.42337]]
输入语法说明：

输入是两个列表：调用成员函数名和调用的参数。Solution 的构造函数有三个参数，圆的半径、圆心的 x 坐标、圆心的 y 坐标。randPoint 没有参数。输入参数是一个列表，即使参数为空，也会输入一个 [] 空列表。

__思路__:

先生成一个极坐标(r, a), 再转换成直角坐标
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    double x = 0, y = 0, r = 0;
    mt19937 rng{random_device{}()};
    uniform_real_distribution<double> uni{0, 1};
public:
    Solution(double radius, double x_center, double y_center) 
    {
        x = x_center;
        y = y_center;
        r = radius;
    }
    
    vector<double> randPoint() 
    {
        double l = r * sqrt(uni(rng)), a = 2 * M_PI * uni(rng);
        return vector<double>{l * cos(a) + x, l * sin(a) + y};
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(radius, x_center, y_center);
 * vector<double> param_1 = obj->randPoint();
 */
```

__Java__:

```Java
class Solution {

    private double x, y, radius;

    public Solution(double radius, double x_center, double y_center) {
        this.x = x_center;
        this.y = y_center;
        this.radius = radius;
    }
    
    public double[] randPoint() {
        double r = Math.sqrt(Math.random() * radius * radius), a = Math.PI * 2 * Math.random();
        return new double[]{r * Math.cos(a) + x, r * Math.sin(a) + y};
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(radius, x_center, y_center);
 * double[] param_1 = obj.randPoint();
 */
```

__Python__:

```Python
class Solution:

    def __init__(self, radius: float, x_center: float, y_center: float):
        self.r = radius
        self.x = x_center
        self.y = y_center


    def randPoint(self) -> List[float]:
        r, a = (random.random() ** 0.5) * self.r, random.uniform(0, 2 * math.pi)
        return [r * math.cos(a) + self.x, r * math.sin(a) + self.y]


# Your Solution object will be instantiated and called as such:
# obj = Solution(radius, x_center, y_center)
# param_1 = obj.randPoint()
```
