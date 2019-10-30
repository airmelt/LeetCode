__Description__:
Given an array A of integers, for each integer A[i] we may choose any x with -K <= x <= K, and add x to A[i].

After this process, we have some array B.

Return the smallest possible difference between the maximum value of B and the minimum value of B.

__Example:__
Example 1:

Input: A = [1], K = 0
Output: 0
Explanation: B = [1]

Example 2:

Input: A = [0,10], K = 2
Output: 6
Explanation: B = [2,8]

Example 3:

Input: A = [1,3,6], K = 3
Output: 0
Explanation: B = [3,3,3] or B = [4,4,4]
 
__Note:__

1 <= A.length <= 10000
0 <= A[i] <= 10000
0 <= K <= 10000

__题目描述__:
给定一个整数数组 A，对于每个整数 A[i]，我们可以选择任意 x 满足 -K <= x <= K，并将 x 加到 A[i] 中。

在此过程之后，我们得到一些数组 B。

返回 B 的最大值和 B 的最小值之间可能存在的最小差值。

__示例 :__
示例 1：

输入：A = [1], K = 0
输出：0
解释：B = [1]

示例 2：

输入：A = [0,10], K = 2
输出：6
解释：B = [2,8]

示例 3：

输入：A = [1,3,6], K = 3
输出：0
解释：B = [3,3,3] 或 B = [4,4,4]
 
__提示：__

1 <= A.length <= 10000
0 <= A[i] <= 10000
0 <= K <= 10000

__思路__:
差距最大的是 max(A)和 min(A), 可以分别向中间靠近
那么就说 max(A) - K和 min(A) + K之间的差距
实际上返回的是 0和 max(A) - min(A) - 2 * K的较大值
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution {
public:
    int smallestRangeI(vector<int>& A, int K) {
        return max(*max_element(A.begin(), A.end()) - *min_element(A.begin(), A.end()) - 2 * K, 0);
    }
};
```

__Java__:
```Java
class Solution {
    public int smallestRangeI(int[] A, int K) {
        int minA = A[0], maxA = A[0];
        for (int num : A) {
            minA = Math.min(minA, num);
            maxA = Math.max(maxA, num);
        }
        return Math.max(maxA - minA - 2 * K, 0);
    }
}
```

__Python__:
```Python
class Solution:
    def smallestRangeI(self, A: List[int], K: int) -> int:
        return max(max(A) - min(A) - 2 * K, 0)
```