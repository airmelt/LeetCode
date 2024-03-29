# 1013 Partition Array Into Three Parts With Equal Sum 将数组分成和相等的三个部分

__Description__:
Given an array A of integers, return true if and only if we can partition the array into three non-empty parts with equal sums.

Formally, we can partition the array if we can find indexes i+1 < j with (A[0] + A[1] + ... + A[i] == A[i+1] + A[i+2] + ... + A[j-1] == A[j] + A[j-1] + ... + A[A.length - 1])

__Example:__

Example 1:

Input: [0,2,1,-6,6,-7,9,1,2,0,1]
Output: true
Explanation: 0 + 2 + 1 = -6 + 6 - 7 + 9 + 1 = 2 + 0 + 1

Example 2:

Input: [0,2,1,-6,6,7,9,-1,2,0,1]
Output: false

Example 3:

Input: [3,3,6,5,-2,2,5,1,-9,4]
Output: true
Explanation: 3 + 3 = 6 = 5 - 2 + 2 + 5 + 1 - 9 + 4

__Note:__

3 <= A.length <= 50000
-10000 <= A[i] <= 10000

__题目描述__:
给定一个整数数组 A，只有我们可以将其划分为三个和相等的非空部分时才返回 true，否则返回 false。

形式上，如果我们可以找出索引 i+1 < j 且满足 (A[0] + A[1] + ... + A[i] == A[i+1] + A[i+2] + ... + A[j-1] == A[j] + A[j-1] + ... + A[A.length - 1]) 就可以将数组三等分。

__示例 :__

示例 1：

输出：[0,2,1,-6,6,-7,9,1,2,0,1]
输出：true
解释：0 + 2 + 1 = -6 + 6 - 7 + 9 + 1 = 2 + 0 + 1

示例 2：

输入：[0,2,1,-6,6,7,9,-1,2,0,1]
输出：false

示例 3：

输入：[3,3,6,5,-2,2,5,1,-9,4]
输出：true
解释：3 + 3 = 6 = 5 - 2 + 2 + 5 + 1 - 9 + 4

__提示：__

3 <= A.length <= 50000
-10000 <= A[i] <= 10000

__思路__:
模拟
找到是否有 3 段相等
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool canThreePartsEqualSum(vector<int>& A) 
    {
        int s = accumulate(A.begin(), A.end(), 0), n = A.size(), cur_sum = 0, count = 0;
        if (s % 3) return false;
        s /= 3;
        for (int i = 0; i < n - 1; i++) 
        {
            cur_sum += A[i];
            if (cur_sum == s) {
                ++count;
                if (count == 2) return true;
                cur_sum = 0;
            }
        }
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canThreePartsEqualSum(int[] A) {
        int s = 0, n = A.length, curSum = 0, count = 0;
        for (int a : A) s += a;
        if (s % 3 != 0) return false;
        s /= 3;
        for (int i = 0; i < n - 1; i++) {
            curSum += A[i];
            if (curSum == s) {
                ++count;
                if (count == 2) return true;
                curSum = 0;
            }
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def canThreePartsEqualSum(self, arr: List[int]) -> bool:
        if (s := sum(arr)) % 3:
            return False
        n, cur_sum, count = len(arr), 0, 0
        s //= 3
        for i in range(n - 1):
            cur_sum += arr[i];
            if cur_sum == s:
                count += 1
                if count == 2:
                    return True
                cur_sum = 0
        return False
```
