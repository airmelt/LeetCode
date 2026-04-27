# 1005 Maximize Sum Of Array After K Negations K 次取反后最大化的数组和

__Description__:
Given an array A of integers, we must modify the array in the following way: we choose an i and replace A[i] with -A[i], and we repeat this process K times in total.  (We may choose the same index i multiple times.)

Return the largest possible sum of the array after modifying it in this way.

__Example:__

Example 1:

Input: A = [4,2,3], K = 1
Output: 5
Explanation: Choose indices (1,) and A becomes [4,-2,3].

Example 2:

Input: A = [3,-1,0,2], K = 3
Output: 6
Explanation: Choose indices (1, 2, 2) and A becomes [3,1,0,2].

Example 3:

Input: A = [2,-3,-1,5,-4], K = 2
Output: 13
Explanation: Choose indices (1, 4) and A becomes [2,3,-1,5,4].

__Note:__

1 <= A.length <= 10000
1 <= K <= 10000
-100 <= A[i] <= 100

__题目描述__:
给定一个整数数组 A，我们只能用以下方法修改该数组：我们选择某个个索引 i 并将 A[i] 替换为 -A[i]，然后总共重复这个过程 K 次。（我们可以多次选择同一个索引 i。）

以这种方式修改数组后，返回数组可能的最大和。

__示例 :__

示例 1：

输入：A = [4,2,3], K = 1
输出：5
解释：选择索引 (1,) ，然后 A 变为 [4,-2,3]。

示例 2：

输入：A = [3,-1,0,2], K = 3
输出：6
解释：选择索引 (1, 2, 2) ，然后 A 变为 [3,1,0,2]。

示例 3：

输入：A = [2,-3,-1,5,-4], K = 2
输出：13
解释：选择索引 (1, 4) ，然后 A 变为 [2,3,-1,5,4]。

__提示：__

1 <= A.length <= 10000
1 <= K <= 10000
-100 <= A[i] <= 100

__思路__:

1. 每次取最小的负数(绝对值最大的负数)反转, 如果数组中全是正数, 取最小的正数反转即可
由于数组中的元素的绝对值均小于等于100, 可以将数组中的元素映射到一个长度为 201的数组中
时间复杂度O(kn), 空间复杂度O(1)
2. 每次先排序, 然后将最小的数取反
时间复杂度O(knlgn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int largestSumAfterKNegations(vector<int>& A, int K) 
    {
        int num[201]{0};
        for (auto a : A) ++num[a + 100];
        int i = 0, result = 0;
        while (K--) 
        {
            while (!num[i]) ++i;
            --num[i];
            ++num[200 - i];
            if (i > 100) i = 200 - i;
        }
        for (; i < 201; i++) result += (i - 100) * num[i];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int largestSumAfterKNegations(int[] A, int K) {
        int num[] = new int[201];
        for (int a : A) num[a + 100]++;
        int i = 0, result = 0;
        while (K-- > 0) {
            while (num[i] == 0) i++;
            num[i]--;
            num[200 - i]++;
            if (i > 100) i = 200 - i;
        }
        for (; i < 201; i++) result += (i - 100) * num[i];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def largestSumAfterKNegations(self, A: List[int], K: int) -> int:
        for _ in range(K):
            A.sort()
            A[0] = -A[0]
        return sum(A)
```
