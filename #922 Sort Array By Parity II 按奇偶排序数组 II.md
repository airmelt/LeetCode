__Description__:
Given an array A of non-negative integers, half of the integers in A are odd, and half of the integers are even.

Sort the array so that whenever A[i] is odd, i is odd; and whenever A[i] is even, i is even.

You may return any answer array that satisfies this condition.

__Example:__
Example 1:

Input: [4,2,5,7]
Output: [4,5,2,7]
Explanation: [4,7,2,5], [2,5,4,7], [2,7,4,5] would also have been accepted.
 
__Note:__

2 <= A.length <= 20000
A.length % 2 == 0
0 <= A[i] <= 1000

__题目描述__:
给定一个非负整数数组 A， A 中一半整数是奇数，一半整数是偶数。

对数组进行排序，以便当 A[i] 为奇数时，i 也是奇数；当 A[i] 为偶数时， i 也是偶数。

你可以返回任何满足上述条件的数组作为答案。

__示例 :__

输入：[4,2,5,7]
输出：[4,5,2,7]
解释：[4,7,2,5]，[2,5,4,7]，[2,7,4,5] 也会被接受。
 
__提示：__

2 <= A.length <= 20000
A.length % 2 == 0
0 <= A[i] <= 1000

__思路__:
双指针法
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution {
public:
    vector<int> sortArrayByParityII(vector<int>& A) 
    {
        int i = 0, j = 1;
        while (i < A.size())
        {
            if ((A[i] & 1))
            {
                while ((A[j] & 1)) j += 2;
                A[i] ^= A[j];
                A[j] ^= A[i];
                A[i] ^= A[j];
            }
            i += 2;
        }
        return A;
    }
};
```

__Java__:
```Java
class Solution {
    public int[] sortArrayByParityII(int[] A) {
        int i = 0, j = 1;
        while (i < A.length) {
            if ((A[i] & 1) != 0) {
                while ((A[j] & 1) != 0) j += 2;
                A[i] ^= A[j];
                A[j] ^= A[i];
                A[i] ^= A[j];
            }
            i += 2;
        }
        return A;
    }
}
```

__Python__:
```Python
class Solution:
    def sortArrayByParityII(self, A: List[int]) -> List[int]:
        return [i for pair in zip([i for i in A if not (i & 1)], [i for i in A if (i & 1)]) for i in pair]
```