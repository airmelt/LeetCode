# 975 Odd Even Jump 奇偶跳

__Description__:
You are given an integer array arr. From some starting index, you can make a series of jumps. The (1st, 3rd, 5th, ...) jumps in the series are called odd-numbered jumps, and the (2nd, 4th, 6th, ...) jumps in the series are called even-numbered jumps. Note that the jumps are numbered, not the indices.

You may jump forward from index i to index j (with i < j) in the following way:

During odd-numbered jumps (i.e., jumps 1, 3, 5, ...), you jump to the index j such that arr[i] <= arr[j] and arr[j] is the smallest possible value. If there are multiple such indices j, you can only jump to the smallest such index j.
During even-numbered jumps (i.e., jumps 2, 4, 6, ...), you jump to the index j such that arr[i] >= arr[j] and arr[j] is the largest possible value. If there are multiple such indices j, you can only jump to the smallest such index j.
It may be the case that for some index i, there are no legal jumps.
A starting index is good if, starting from that index, you can reach the end of the array (index arr.length - 1) by jumping some number of times (possibly 0 or more than once).

Return the number of good starting indices.

__Example:__

Example 1:

Input: arr = [10,13,12,14,15]
Output: 2
Explanation:
From starting index i = 0, we can make our 1st jump to i = 2 (since arr[2] is the smallest among arr[1], arr[2], arr[3], arr[4] that is greater or equal to arr[0]), then we cannot jump any more.
From starting index i = 1 and i = 2, we can make our 1st jump to i = 3, then we cannot jump any more.
From starting index i = 3, we can make our 1st jump to i = 4, so we have reached the end.
From starting index i = 4, we have reached the end already.
In total, there are 2 different starting indices i = 3 and i = 4, where we can reach the end with some number of
jumps.

Example 2:

Input: arr = [2,3,1,1,4]
Output: 3
Explanation:
From starting index i = 0, we make jumps to i = 1, i = 2, i = 3:
During our 1st jump (odd-numbered), we first jump to i = 1 because arr[1] is the smallest value in [arr[1], arr[2], arr[3], arr[4]] that is greater than or equal to arr[0].
During our 2nd jump (even-numbered), we jump from i = 1 to i = 2 because arr[2] is the largest value in [arr[2], arr[3], arr[4]] that is less than or equal to arr[1]. arr[3] is also the largest value, but 2 is a smaller index, so we can only jump to i = 2 and not i = 3
During our 3rd jump (odd-numbered), we jump from i = 2 to i = 3 because arr[3] is the smallest value in [arr[3], arr[4]] that is greater than or equal to arr[2].
We can't jump from i = 3 to i = 4, so the starting index i = 0 is not good.
In a similar manner, we can deduce that:
From starting index i = 1, we jump to i = 4, so we reach the end.
From starting index i = 2, we jump to i = 3, and then we can't jump anymore.
From starting index i = 3, we jump to i = 4, so we reach the end.
From starting index i = 4, we are already at the end.
In total, there are 3 different starting indices i = 1, i = 3, and i = 4, where we can reach the end with some
number of jumps.

Example 3:

Input: arr = [5,1,3,4,2]
Output: 3
Explanation: We can reach the end from starting indices 1, 2, and 4.

__Constraints:__

1 <= arr.length <= 2 * 10^4
0 <= arr[i] < 10^5

__题目描述__:
给定一个整数数组 A，你可以从某一起始索引出发，跳跃一定次数。在你跳跃的过程中，第 1、3、5... 次跳跃称为奇数跳跃，而第 2、4、6... 次跳跃称为偶数跳跃。

你可以按以下方式从索引 i 向后跳转到索引 j（其中 i < j）：

在进行奇数跳跃时（如，第 1，3，5... 次跳跃），你将会跳到索引 j，使得 A[i] <= A[j]，A[j] 是可能的最小值。如果存在多个这样的索引 j，你只能跳到满足要求的最小索引 j 上。
在进行偶数跳跃时（如，第 2，4，6... 次跳跃），你将会跳到索引 j，使得 A[i] >= A[j]，A[j] 是可能的最大值。如果存在多个这样的索引 j，你只能跳到满足要求的最小索引 j 上。
（对于某些索引 i，可能无法进行合乎要求的跳跃。）
如果从某一索引开始跳跃一定次数（可能是 0 次或多次），就可以到达数组的末尾（索引 A.length - 1），那么该索引就会被认为是好的起始索引。

返回好的起始索引的数量。

__示例 :__

示例 1：

输入：[10,13,12,14,15]
输出：2
解释：
从起始索引 i = 0 出发，我们可以跳到 i = 2，（因为 A[2] 是 A[1]，A[2]，A[3]，A[4] 中大于或等于 A[0] 的最小值），然后我们就无法继续跳下去了。
从起始索引 i = 1 和 i = 2 出发，我们可以跳到 i = 3，然后我们就无法继续跳下去了。
从起始索引 i = 3 出发，我们可以跳到 i = 4，到达数组末尾。
从起始索引 i = 4 出发，我们已经到达数组末尾。
总之，我们可以从 2 个不同的起始索引（i = 3, i = 4）出发，通过一定数量的跳跃到达数组末尾。

示例 2：

输入：[2,3,1,1,4]
输出：3
解释：
从起始索引 i=0 出发，我们依次可以跳到 i = 1，i = 2，i = 3：

在我们的第一次跳跃（奇数）中，我们先跳到 i = 1，因为 A[1] 是（A[1]，A[2]，A[3]，A[4]）中大于或等于 A[0] 的最小值。

在我们的第二次跳跃（偶数）中，我们从 i = 1 跳到 i = 2，因为 A[2] 是（A[2]，A[3]，A[4]）中小于或等于 A[1] 的最大值。A[3] 也是最大的值，但 2 是一个较小的索引，所以我们只能跳到 i = 2，而不能跳到 i = 3。

在我们的第三次跳跃（奇数）中，我们从 i = 2 跳到 i = 3，因为 A[3] 是（A[3]，A[4]）中大于或等于 A[2] 的最小值。

我们不能从 i = 3 跳到 i = 4，所以起始索引 i = 0 不是好的起始索引。

类似地，我们可以推断：
从起始索引 i = 1 出发， 我们跳到 i = 4，这样我们就到达数组末尾。
从起始索引 i = 2 出发， 我们跳到 i = 3，然后我们就不能再跳了。
从起始索引 i = 3 出发， 我们跳到 i = 4，这样我们就到达数组末尾。
从起始索引 i = 4 出发，我们已经到达数组末尾。
总之，我们可以从 3 个不同的起始索引（i = 1, i = 3, i = 4）出发，通过一定数量的跳跃到达数组末尾。

示例 3：

输入：[5,1,3,4,2]
输出：3
解释：
我们可以从起始索引 1，2，4 出发到达数组末尾。

__提示:__

1 <= A.length <= 20000
0 <= A[i] < 100000

__思路__:

1. 单调栈
由于需要找到第一个大于或等于自身得下标得位置
先将下标按照 arr 中得相对顺序进行排序
使用单调栈预处理得到每一个位置对应得下一个位置
其中奇数跳和偶数跳对应得大于小于可以用负号代替
然后从后往前遍历, 模拟跳跃即可
时间复杂度为 O(nlgn), 空间复杂度为 O(n)
2. 二分查找
利用 TreeMap 或者 map 二分查找
分别按照奇偶跳跃找到下一次跳跃得位置
如果当前能跳到终点, 结果自增
时间复杂度为 O(nlgn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int oddEvenJumps(vector<int>& arr) 
    {
        int n = arr.size(), result = 1;
        vector<vector<bool>> dp(n, vector<bool>(2, false));
        dp.back().front() = dp.back().back() = true;
        map<int, int> m;
        m[arr.back()] = n - 1;
        for (int i = n - 2; i > -1; --i)
        {
            auto it = m.lower_bound(arr[i]);
            if (it != m.end()) dp[i][1] = dp[it -> second][0];
            it = m.upper_bound(arr[i]);
            if (it-- != m.begin()) dp[i][0] = dp[it -> second][1];
            m[arr[i]] = i;
            if (dp[i][1]) ++result;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int oddEvenJumps(int[] arr) {
        int n = arr.length, result = 0;
        TreeMap<Integer, Integer> map = new TreeMap<>();
        boolean[][] dp = new boolean[n][2];
        dp[n - 1][0] = dp[n - 1][1] = true;
        for (int i = n - 1; i >= 0; -- i) {
            Integer p1 = map.ceilingKey(arr[i]), p2 = map.floorKey(arr[i]);
            if (p1 != null) dp[i][0] |= dp[map.get(p1)][1];
            if (p2 != null) dp[i][1] |= dp[map.get(p2)][0];
            map.put(arr[i], i);
            if (dp[i][0]) ++result;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def oddEvenJumps(self, arr: List[int]) -> int:
        n = len(arr)
        
        def helper(nums: List[int]) -> List[int]:
            result, stack = [None] * n, []
            for num in nums:
                while stack and num > stack[-1]:
                    result[stack.pop()] = num
                stack.append(num)
            return result
        odd_next, even_next, odd, even = helper(sorted(range(n), key=lambda num: arr[num])), helper(sorted(range(n), key=lambda num: -arr[num])), [False] * (n - 1) + [True], [False] * (n - 1) + [True]
        for i in range(n - 2, -1, -1):
            odd[i], even[i] = even[odd_next[i]] if odd_next[i] is not None else odd[i], odd[even_next[i]] if even_next[i] is not None else even[i]
        return sum(odd)
```
