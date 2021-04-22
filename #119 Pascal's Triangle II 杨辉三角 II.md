# 119 Pascal's Triangle II 杨辉三角 II

__Description__:
Given a non-negative index *k* where *k* ≤ 33, return the *kth* index row of the Pascal's triangle.

Note that the row index starts from 0.

![In Pascal's triangle, each number is the sum of the two numbers directly above it.](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)

**Example:**

**Input:** 3
**Output:** [1,3,3,1]

**Follow up:**
Could you optimize your algorithm to use only *O*(*k*) extra space?

__题目描述__:
给定一个非负索引 *k*，其中 *k* ≤ 33，返回杨辉三角的第 *k*行。

![在杨辉三角中，每个数是它左上方和右上方的数的和。](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)

**示例:**

**输入:** 3
**输出:** [1,3,3,1]

**进阶：**
你可以优化你的算法到 *O*(*k*) 空间复杂度吗？

__思路__:

1. 参考[LeetCode #118 Pascal's Triangle 杨辉三角](https://www.jianshu.com/p/0e4ed3b38e4c), 取某一行结果
2. 通项公式为 C(n, k) = n! / [k! * (n - k)!] , 只需计算前一半, 每一层均是对称的
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> getRow(int rowIndex) 
    {
        vector<int> result;
        long temp = 1;
        for (int i = 0; i < rowIndex + 1; i++) 
        {
            result.push_back((int)temp);
            /* 注意这里一定不能写成 temp *= (rowIndex - i) / (i + 1)
             * 因为 *= 是乘法赋值运算符, (rowIndex - i) / (i + 1) 会先进行运算
             * 实际上应当 temp 先与 (rowIndex - i) 相乘再去除 (i + 1)
             * 可以写成 temp *= (rowIndex - i); temp /= (i + 1)
             */
            temp = temp * (rowIndex - i) / (i + 1);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> getRow(int rowIndex) {
        List<Integer> result = new ArrayList<>(rowIndex + 1);
        long temp = 1;
        for (int i = 0; i <= rowIndex; i++) {
            result.add((int)temp);
            temp = temp * (rowIndex - i) / (i + 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def getRow(self, rowIndex: int) -> List[int]:
        row = [1]
        while rowIndex:
            row = [sum(i) for i in zip([0] + row, row + [0])]
            rowIndex -= 1
        return row
```
