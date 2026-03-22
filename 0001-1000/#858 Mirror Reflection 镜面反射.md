# 858 Mirror Reflection 镜面反射

__Description__:
There is a special square room with mirrors on each of the four walls. Except for the southwest corner, there are receptors on each of the remaining corners, numbered 0, 1, and 2.

The square room has walls of length p and a laser ray from the southwest corner first meets the east wall at a distance q from the 0th receptor.

Given the two integers p and q, return the number of the receptor that the ray meets first.

The test cases are guaranteed so that the ray will meet a receptor eventually.

__Example:__

Example 1:

![Mirror Reflection](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/06/18/reflection.png)

Input: p = 2, q = 1
Output: 2
Explanation: The ray meets receptor 2 the first time it gets reflected back to the left wall.

Example 2:

Input: p = 3, q = 1
Output: 1

__Constraints:__

1 <= q <= p <= 1000

__题目描述__:
有一个特殊的正方形房间，每面墙上都有一面镜子。除西南角以外，每个角落都放有一个接受器，编号为 0， 1，以及 2。

正方形房间的墙壁长度为 p，一束激光从西南角射出，首先会与东墙相遇，入射点到接收器 0 的距离为 q 。

返回光线最先遇到的接收器的编号（保证光线最终会遇到一个接收器）。

__示例 :__

![镜面反射](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/06/22/reflection.png)

输入： p = 2, q = 1
输出： 2
解释： 这条光线在第一次被反射回左边的墙时就遇到了接收器 2 。

__提示:__

1 <= p <= 1000
0 <= q <= p

__思路__:

数学
将正方形扩展为 pq * pq 的大小的正方形
不考虑光的反射, 将光看作直线, 斜率为 q / p, 转化为找到这条直线的第一个整数点
q 为偶数必然落在点 0
p 为偶数必然落在点 2
p, q 均为奇数必然落在点 1
所以将两个数同时含有的 2 的因子除去按照就判断即可
时间复杂度为 O(lgn), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int mirrorReflection(int p, int q)
    {
        while (!(p & 1) and !(q & 1)) 
        {
            p >>= 1;
            q >>= 1;
        }
        return !(p & 1) ? 2 : (!(q & 1) ? 0 : 1);
    }
};
```

__Java__:

```Java
class Solution {
    public int mirrorReflection(int p, int q) {
        while ((p & 1) == 0 && (q & 1) == 0) {
            p >>>= 1;
            q >>>= 1;
        }
        return (p & 1) == 0 ? 2 : ((q & 1) == 0 ? 0 : 1);
    }
}
```

__Python__:

```Python
class Solution:
    def mirrorReflection(self, p: int, q: int) -> int:
        while (not (p & 1)) and (not (q & 1)):
            p >>= 1
            q >>= 1
        return 2 if not (p & 1) else 0 if not (q & 1) else 1
```
