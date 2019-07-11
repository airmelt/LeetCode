__Description__:
The Hamming distance between two integers is the number of positions at which the corresponding bits are different.

Given two integers x and y, calculate the Hamming distance.

__Note:__
0 ≤ x, y < 231.

__Example:__

Input: x = 1, y = 4

Output: 2

Explanation:
```
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑
```
The above arrows point to positions where the corresponding bits are different.

__题目描述__:
两个整数之间的汉明距离指的是这两个数字对应二进制位不同的位置的数目。

给出两个整数 x 和 y，计算它们之间的汉明距离。

__注意：__
0 ≤ x, y < 231.

__示例:__

输入: x = 1, y = 4

输出: 2

解释:
```
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑
```
上面的箭头指出了对应二进制位不同的位置。

__思路__:
先用异或, 计算 1的数量可以用 num & (num - 1)来统计
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    int hammingDistance(int x, int y) {
        // 也可以用 bitset类
        int result = 0, num = x ^ y;
        while (num) {
            num = (num - 1) & num;
            result++;
        }
        return result;
    }
};
```

__Java__:
```
class Solution {
    public int hammingDistance(int x, int y) {
        return Integer.toBinaryString(x ^ y).replaceAll("0", "").length();
    }
}
```

__Python__:
```
class Solution:
    def hammingDistance(self, x: int, y: int) -> int:
        return bin(x ^ y).count("1")
```
