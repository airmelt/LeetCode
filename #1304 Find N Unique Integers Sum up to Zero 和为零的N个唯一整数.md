__Description__:
Given an integer n, return any array containing n unique integers such that they add up to 0.

__Example:__
Example 1:

Input: n = 5
Output: [-7,-1,1,3,4]
Explanation: These arrays also are accepted [-5,-1,1,2,3] , [-3,-1,2,-2,4].

Example 2:

Input: n = 3
Output: [-1,0,1]

Example 3:

Input: n = 1
Output: [0]
 
__Constraints:__

1 <= n <= 1000

__题目描述__:
给你一个整数 n，请你返回 任意 一个由 n 个 各不相同 的整数组成的数组，并且这 n 个数相加和为 0 。

__示例 :__
示例 1：

输入：n = 5
输出：[-7,-1,1,3,4]
解释：这些数组也是正确的 [-5,-1,1,2,3]，[-3,-1,2,-2,4]。

示例 2：

输入：n = 3
输出：[-1,0,1]

示例 3：

输入：n = 1
输出：[0]
 
__提示：__

1 <= n <= 1000

__思路__:
从 1 - n到 n - 1按照步长为 2加入到结果数组即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<int> sumZero(int n) 
    {
        vector<int> result(n);
        for (int num = 1 - n, i = 0; num < n; num += 2, i++) result[i] = num;
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int[] sumZero(int n) {
        int result[] = new int[n];
        for (int i = 0, num = 1 - n; i < n; i++, num += 2) result[i] = num;
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def sumZero(self, n: int) -> List[int]:
        return range(1 - n, n, 2)
```