__题目描述__:
有一个同学在学习分式。他需要将一个连分数化成最简分数，你能帮助他吗？
![连分数](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/09/09/fraction_example_1.jpg)
连分数是形如上图的分式。在本题中，所有系数都是大于等于0的整数。

输入的cont代表连分数的系数（cont[0]代表上图的a0，以此类推）。返回一个长度为2的数组[n, m]，使得连分数的值等于n / m，且n, m最大公约数为1。

__示例 :__
示例 1：

输入：cont = [3, 2, 0, 2]
输出：[13, 4]
解释：原连分数等价于3 + (1 / (2 + (1 / (0 + 1 / 2))))。注意[26, 8], [-13, -4]都不是正确答案。

示例 2：

输入：cont = [0, 0, 3]
输出：[3, 1]
解释：如果答案是整数，令分母为1即可。

__限制：__

cont[i] >= 0
1 <= cont的长度 <= 10
cont最后一个元素不等于0
答案的n, m的取值都能被32位int整型存下（即不超过2 ^ 31 - 1）。

__思路__:
a + 1 / b 等于 (ab + 1) / b已经是一个最简分数, 从数组的最右开始往左遍历, 不断交换分母分子, 下一次的分母就是上一次的分子
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<int> fraction(vector<int>& cont) 
    {
        int a = cont.back(), b = 1;
        for (auto iter = cont.rbegin() + 1; iter != cont.rend(); iter++)
        {
            swap(a, b);
            a += (*iter) * b;
        }
        return {a, b};
    }
};
```

__Java__:
```Java
class Solution {
    public int[] fraction(int[] cont) {
        int a = cont[cont.length - 1], b = 1;
        for (int i = cont.length - 2; i > -1; i--) {
            int temp = a;
            a = b;
            b = temp;
            a += cont[i] * b;
        }
        return new int[]{a, b};
    }
}
```

__Python__:
```Python
class Solution:
    def fraction(self, cont: List[int]) -> List[int]:
        a, b = cont[-1], 1
        for i in range(len(cont) - 2, -1, -1):
            a, b = b, a
            a += cont[i] * b
        return [a, b]
```