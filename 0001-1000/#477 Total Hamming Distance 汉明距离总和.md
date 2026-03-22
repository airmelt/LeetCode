# 477 Total Hamming Distance 汉明距离总和

__Description__:
The Hamming distance between two integers is the number of positions at which the corresponding bits are different.

Now your job is to find the total Hamming distance between all pairs of the given numbers.

__Example:__

Input: 4, 14, 2

Output: 6

Explanation: In binary representation, the 4 is 0100, 14 is 1110, and 2 is 0010 (just
showing the four bits relevant in this case). So the answer will be:
HammingDistance(4, 14) + HammingDistance(4, 2) + HammingDistance(14, 2) = 2 + 2 + 2 = 6.

__Note:__
Elements of the given array are in the range of 0 to 10^9
Length of the array will not exceed 10^4.

__题目描述__:
两个整数的 汉明距离 指的是这两个数字的二进制数对应位不同的数量。

计算一个数组中，任意两个数之间汉明距离的总和。

__示例 :__

输入: 4, 14, 2

输出: 6

解释: 在二进制表示中，4表示为0100，14表示为1110，2表示为0010。（这样表示是为了体现后四位之间关系）
所以答案为：
HammingDistance(4, 14) + HammingDistance(4, 2) + HammingDistance(14, 2) = 2 + 2 + 2 = 6.

__注意:__

数组中元素的范围为从 0到 10^9。
数组的长度不超过 10^4。

__思路__:

位运算
将数字转换为二进制,
比如 4 -> 0100, 14 -> 1110, 2 -> 0010,
对于每一位的数字, 如果全 1或者全 0, 则汉明距离为 0,
如果有不同的数字, 则每个 0和每个 1两两可以产生 1个汉明距离,
比如 4, 14, 2的最高位分别为 0, 1, 0, 这里可以产生 2个汉明距离,
所以对每一位计算出 1的个数, 乘上 0个个数就可以得到该位置的汉明距离, 总和就是数组的汉明距离
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int totalHammingDistance(vector<int>& nums) 
    {
        int result = 0, n = nums.size();
        for (int i = 0; i < 32; i++) 
        {
            int one = 0;
            for (auto &num: nums) one += num >> i & 1;
            result += (n - one) * one;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int totalHammingDistance(int[] nums) {
        int result = 0, n = nums.length;
        for (int i = 0; i < 32; i++) {
            int one = 0;
            for (int num : nums) one += num >> i & 1;
            result += (n - one) * one;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def totalHammingDistance(self, nums: List[int]) -> int:
        return sum((one := v.count('1')) * (len(nums) - one) for v in zip(*map('{:032b}'.format, nums)))
```
