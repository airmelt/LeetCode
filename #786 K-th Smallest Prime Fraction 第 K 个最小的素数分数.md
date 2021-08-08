# 786 K-th Smallest Prime Fraction 第 K 个最小的素数分数

__Description__:
You are given a sorted integer array arr containing 1 and prime numbers, where all the integers of arr are unique. You are also given an integer k.

For every i and j where 0 <= i < j < arr.length, we consider the fraction arr[i] / arr[j].

Return the kth smallest fraction considered. Return your answer as an array of integers of size 2, where answer[0] == arr[i] and answer[1] == arr[j].

__Example:__

Example 1:

Input: arr = [1,2,3,5], k = 3
Output: [2,5]
Explanation: The fractions to be considered in sorted order are:
1/5, 1/3, 2/5, 1/2, 3/5, and 2/3.
The third fraction is 2/5.

Example 2:

Input: arr = [1,7], k = 1
Output: [1,7]

__Constraints:__

2 <= arr.length <= 1000
1 <= arr[i] <= 3 \* 10^4
arr[0] == 1
arr[i] is a prime number for i > 0.
All the numbers of arr are unique and sorted in strictly increasing order.
1 <= k <= arr.length \* (arr.length - 1) / 2

__题目描述__:
给你一个按递增顺序排序的数组 arr 和一个整数 k 。数组 arr 由 1 和若干 素数  组成，且其中所有整数互不相同。

对于每对满足 0 < i < j < arr.length 的 i 和 j ，可以得到分数 arr[i] / arr[j] 。

那么第 k 个最小的分数是多少呢?  以长度为 2 的整数数组返回你的答案, 这里 answer[0] == arr[i] 且 answer[1] == arr[j] 。

__示例 :__

示例 1：

输入：arr = [1,2,3,5], k = 3
输出：[2,5]
解释：已构造好的分数,排序后如下所示:
1/5, 1/3, 2/5, 1/2, 3/5, 2/3
很明显第三个最小的分数是 2/5

示例 2：

输入：arr = [1,7], k = 1
输出：[1,7]

__提示:__

2 <= arr.length <= 1000
1 <= arr[i] <= 3 \* 10^4
arr[0] == 1
arr[i] 是一个 素数 ，i > 0
arr 中的所有数字 互不相同 ，且按 严格递增 排序
1 <= k <= arr.length \* (arr.length - 1) / 2

__思路__:

1. 优先队列
放入前 k 个素数分数到优先队列即可, 分数可以转换为乘法求出其相对大小
a / b < c / d -> a \* d < b \* c
时间复杂度为 O(nlgk), 空间复杂度为 O(k)
2. 二分法
在 0 - 1 中用二分查找出 count == k 的值即可
时间复杂度为 O(nlgn), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> kthSmallestPrimeFraction(vector<int>& arr, int k) 
    {
        auto cmp = [] (pair<int, int>& a, pair<int, int>& b) { return a.first * b.second < a.second * b.first; };
        priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(cmp)> pq(cmp);
        int n = arr.size();
        for (int i = 1; i < n; i++) 
        {
            for (int j = 0; j < i; j++) 
            {
                pq.emplace(arr[j], arr[i]);
                if (pq.size() > k) pq.pop();
            }
        }
        return vector<int>{ pq.top().first, pq.top().second };
    }
};
```

__Java__:

```Java
class Solution {
    public int[] kthSmallestPrimeFraction(int[] arr, int k) {
        double left = 0, right = 1.0, mid = 0.0;
        int p = 0, q = 1, n = arr.length, count = 0;
        while (true) {
            mid = (left + right) / 2.0;
            count = 0;
            p = 0;
            for (int i = 0, j = 0; i < n; i++) {
                while (j < n && arr[i] > mid * arr[j]) ++j;
                count += n - j;
                if (j < n && p * arr[j] < q * arr[i]) {
                    p = arr[i];
                    q = arr[j];
                }
            }
            if (count == k) return new int[]{ p, q };
            else if (count < k) left = mid;
            else right = mid;
        }
    }
}
```

__Python__:

```Python
class Solution:
    def kthSmallestPrimeFraction(self, arr: List[int], k: int) -> List[int]:
        pq = [(arr[0] / float(arr[i]), 0, i) for i in range(len(arr) - 1, 0, -1)]
        for _ in range(k - 1):
            frac, i, j = heapq.heappop(pq)
            i += 1
            if i < j:
                heapq.heappush(pq, (arr[i] / float(arr[j]), i, j))
        return arr[pq[0][1]], arr[pq[0][2]]
```
