# 3200 Maximum Height of a Triangle 三角形的最大高度

__Description:__

You are given two integers `red` and `blue` representing the count of red and blue colored balls. You have to arrange these balls to form a triangle such that the 1st row will have 1 ball, the 2nd row will have 2 balls, the 3rd row will have 3 balls, and so on.

All the balls in a particular row should be the __same__ color, and adjacent rows should have __different__ colors.

Return the __maximum__ _height of the triangle_ that can be achieved.

__Example:__

Example 1:

```text
Input: red = 2, blue = 4

Output: 3

Explanation:
```

![3200-1](https://assets.leetcode.com/uploads/2024/06/16/brb.png)

The only possible arrangement is shown above.

Example 2:

```text
Input: red = 2, blue = 1

Output: 2

Explanation:
```

![3200-2](https://assets.leetcode.com/uploads/2024/06/16/br.png)

The only possible arrangement is shown above.

Example 3:

```text
Input: red = 1, blue = 1

Output: 1
```

Example 4:

```text
Input: red = 10, blue = 1

Output: 2

Explanation:
```

![3200-3](https://assets.leetcode.com/uploads/2024/06/16/br.png)

The only possible arrangement is shown above.

__Constraints:__

- `1 <= red, blue <= 100`

__题目描述:__

给你两个整数 `red` 和 `blue`，分别表示红色球和蓝色球的数量。你需要使用这些球来组成一个三角形，满足第 1 行有 1 个球，第 2 行有 2 个球，第 3 行有 3 个球，依此类推。

每一行的球必须是 __相同__ 颜色，且相邻行的颜色必须 __不同__。

返回可以实现的三角形的 __最大__ 高度。

__示例:__

示例 1：

```text
输入： red = 2, blue = 4

输出： 3

解释：
```

![3200-4](https://assets.leetcode.com/uploads/2024/06/16/brb.png)

上图显示了唯一可能的排列方式。

示例 2：

```text
输入： red = 2, blue = 1

输出： 2

解释：
```

![3200-5](https://assets.leetcode.com/uploads/2024/06/16/br.png)

上图显示了唯一可能的排列方式。

示例 3：

```text
输入： red = 1, blue = 1

输出： 1
```

示例 4：

```text
输入： red = 10, blue = 1

输出： 2

解释：
```

![3200-6](https://assets.leetcode.com/uploads/2024/06/16/br.png)

上图显示了唯一可能的排列方式。

__提示：__

- `1 <= red, blue <= 100`

__思路:__

```text
2. 数学
对于奇数行如果有 k 行应该为 1 + 3 + ... + (k * 2 - 1) = k ^ 2
对于偶数行如果有 k 行应该为 2 + 4 + ... + k * 2 = k ^ 2 + 2
k ^ 2 <= n 且 k ^ 2 + 2 <= n
分别解方程可得 k <= sqrt(n), k <= (sqrt(n * 4 + 1) - 1) / 2
记录 odd 为奇数行, even 为偶数行
如果 odd > even 那么一共有 2 * even + 1 行
否则一共有 2 * odd 行
时间复杂度为 O(1), 空间复杂度为 O(1), 计算平方根视为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxHeightOfTriangle(int red, int blue) 
    {
        int c[2]{};
        for (int i = 1; ; i++) 
        {
            c[i & 1] += i;
            if ((c[0] > red or c[1] > blue) and (c[1] > red or c[0] > blue)) return i - 1;
        }
    }
};
```

__Java__:

```Java
class Solution {
    public int maxHeightOfTriangle(int red, int blue) {
        int c[] = new int[2];
        for (int i = 1; ; i++) {
            c[i & 1] += i;
            if ((c[0] > red || c[1] > blue) && (c[1] > red || c[0] > blue)) return i - 1;
        }
    }
}
```

__Python__:

```Python
class Solution:
    def maxHeightOfTriangle(self, red: int, blue: int) -> int:
        return max((int((sqrt((red << 2) + 1) - 1) / 2) << 1) + 1 if isqrt(blue) > int((sqrt((red << 2) + 1) - 1) / 2) else isqrt(blue) << 1, (int((sqrt((blue << 2) + 1) - 1) / 2) << 1) + 1 if isqrt(red) > int((sqrt((blue << 2) + 1) - 1) / 2) else isqrt(red) << 1)
```
