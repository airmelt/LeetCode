__Description__:
Alice and Bob have candy bars of different sizes: A[i] is the size of the i-th bar of candy that Alice has, and B[j] is the size of the j-th bar of candy that Bob has.

Since they are friends, they would like to exchange one candy bar each so that after the exchange, they both have the same total amount of candy.  (The total amount of candy a person has is the sum of the sizes of candy bars they have.)

Return an integer array ans where ans[0] is the size of the candy bar that Alice must exchange, and ans[1] is the size of the candy bar that Bob must exchange.

If there are multiple answers, you may return any one of them.  It is guaranteed an answer exists.

__Example:__
Example 1:

Input: A = [1,1], B = [2,2]
Output: [1,2]
Example 2:

Input: A = [1,2], B = [2,3]
Output: [1,2]
Example 3:

Input: A = [2], B = [1,3]
Output: [2,3]
Example 4:

Input: A = [1,2,5], B = [2,4]
Output: [5,4]
 
__Note:__

1 <= A.length <= 10000
1 <= B.length <= 10000
1 <= A[i] <= 100000
1 <= B[i] <= 100000
It is guaranteed that Alice and Bob have different total amounts of candy.
It is guaranteed there exists an answer.

__题目描述__:
爱丽丝和鲍勃有不同大小的糖果棒：A[i] 是爱丽丝拥有的第 i 块糖的大小，B[j] 是鲍勃拥有的第 j 块糖的大小。

因为他们是朋友，所以他们想交换一个糖果棒，这样交换后，他们都有相同的糖果总量。（一个人拥有的糖果总量是他们拥有的糖果棒大小的总和。）

返回一个整数数组 ans，其中 ans[0] 是爱丽丝必须交换的糖果棒的大小，ans[1] 是 Bob 必须交换的糖果棒的大小。

如果有多个答案，你可以返回其中任何一个。保证答案存在。

__示例 :__
示例 1：

输入：A = [1,1], B = [2,2]
输出：[1,2]

示例 2：

输入：A = [1,2], B = [2,3]
输出：[1,2]

示例 3：

输入：A = [2], B = [1,3]
输出：[2,3]

示例 4：

输入：A = [1,2,5], B = [2,4]
输出：[5,4]
 
__提示：__

1 <= A.length <= 10000
1 <= B.length <= 10000
1 <= A[i] <= 100000
1 <= B[i] <= 100000
保证爱丽丝与鲍勃的糖果总量不同。
答案肯定存在。

__思路__:
返回的值应该是 A数组中的一个存在于 B数组中并且满足 num + (sum(B) - sum(A)) // 2的数
注意要把 B数组转化成 set, 查找时间为 O(1)
时间复杂度O(n), 空间复杂度O(n), 其中 n为两数组的最长长度

__代码__:
__C++__:
```C++
class Solution {
public:
    vector<int> fairCandySwap(vector<int>& A, vector<int>& B) {
        int sum_A = accumulate(A.begin(), A.end(), 0), sum_B = accumulate(B.begin(), B.end(), 0);
        set<int> set_B(B.begin(), B.end());
        for (int num : A) if (set_B.count(num + ((sum_B - sum_A) >> 1))) return {num, num + ((sum_B - sum_A) >> 1)};
        return {};
    }
};
```

__Java__:
```Java
class Solution {
    public int[] fairCandySwap(int[] A, int[] B) {
        int sumA = 0, sumB = 0;
        Set<Integer> setB = new HashSet<>(B.length);
        for (int a : A) sumA += a;
        for (int b : B) {
            sumB += b;
            setB.add(b);
        }
        for (int num : A) if (setB.contains(num + ((sumB - sumA) >> 1))) return new int[]{num, num + ((sumB - sumA) >> 1)};
        return new int[2];
    }
}
```

__Python__:
```Python
class Solution:
    def fairCandySwap(self, A: List[int], B: List[int]) -> List[int]:
        sum_A, sum_B = sum(A), sum(B)
        set_B = set(B)
        for num in A:
            if num + ((sum_B - sum_A) >> 1) in set_B:
                return [num, num + ((sum_B - sum_A) >> 1)]
```