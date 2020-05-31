__Description__:
The gray code is a binary numeral system where two successive values differ in only one bit.

Given a non-negative integer n representing the total number of bits in the code, print the sequence of gray code. A gray code sequence must begin with 0.

__Example:__
Example 1:

Input: 2
Output: [0,1,3,2]
Explanation:
00 - 0
01 - 1
11 - 3
10 - 2

For a given n, a gray code sequence may not be uniquely defined.
For example, [0,2,3,1] is also a valid gray code sequence.

00 - 0
10 - 2
11 - 3
01 - 1

Example 2:

Input: 0
Output: [0]
Explanation: We define the gray code sequence to begin with 0.
             A gray code sequence of n has size = 2n, which for n = 0 the size is 20 = 1.
             Therefore, for n = 0 the gray code sequence is [0].

__题目描述__:
格雷编码是一个二进制数字系统，在该系统中，两个连续的数值仅有一个位数的差异。

给定一个代表编码总位数的非负整数 n，打印其格雷编码序列。即使有多个不同答案，你也只需要返回其中一种。

格雷编码序列必须以 0 开头。

__示例 :__
示例 1:

输入: 2
输出: [0,1,3,2]
解释:
00 - 0
01 - 1
11 - 3
10 - 2

对于给定的 n，其格雷编码序列并不唯一。
例如，[0,2,3,1] 也是一个有效的格雷编码序列。

00 - 0
10 - 2
11 - 3
01 - 1

示例 2:

输入: 0
输出: [0]
解释: 我们定义格雷编码序列必须以 0 开头。
     给定编码总位数为 n 的格雷编码序列，其长度为 2n。当 n = 0 时，长度为 20 = 1。
     因此，当 n = 0 时，其格雷编码序列为 [0]。

__思路__:
格雷码生成方式 i ^ (i >> 1)
时间复杂度O(2 ^ n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<int> grayCode(int n) 
    {
        vector<int> result((int)pow(2, n), 0);
        for (int i = 0; i < 1 << n; i++) result[i] = i ^ i >> 1;
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public List<Integer> grayCode(int n) {
        List<Integer> result = new ArrayList<>((int)Math.pow(2, n));
        for (int i = 0; i < 1 << n; i++) result.add(i ^ i >> 1);
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def grayCode(self, n: int) -> List[int]:
        return list(i ^ i >> 1 for i in range(1 << n))
```