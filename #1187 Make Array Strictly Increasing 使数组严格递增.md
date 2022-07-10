# 1187 Make Array Strictly Increasing 使数组严格递增

__Description__:
Given two integer arrays arr1 and arr2, return the minimum number of operations (possibly zero) needed to make arr1 strictly increasing.

In one operation, you can choose two indices 0 <= i < arr1.length and 0 <= j < arr2.length and do the assignment arr1[i] = arr2[j].

If there is no way to make arr1 strictly increasing, return -1.

__Example:__

Example 1:

Input: arr1 = [1,5,3,6,7], arr2 = [1,3,2,4]
Output: 1
Explanation: Replace 5 with 2, then arr1 = [1, 2, 3, 6, 7].

Example 2:

Input: arr1 = [1,5,3,6,7], arr2 = [4,3,1]
Output: 2
Explanation: Replace 5 with 3 and then replace 3 with 4. arr1 = [1, 3, 4, 6, 7].

Example 3:

Input: arr1 = [1,5,3,6,7], arr2 = [1,6,3,3]
Output: -1
Explanation: You can't make arr1 strictly increasing.

__Constraints:__

1 <= arr1.length, arr2.length <= 2000
0 <= arr1[i], arr2[i] <= 10^9

__题目描述__:
给你两个整数数组 arr1 和 arr2，返回使 arr1 严格递增所需要的最小「操作」数（可能为 0）。

每一步「操作」中，你可以分别从 arr1 和 arr2 中各选出一个索引，分别为 i 和 j，0 <= i < arr1.length 和 0 <= j < arr2.length，然后进行赋值运算 arr1[i] = arr2[j]。

如果无法让 arr1 严格递增，请返回 -1。

__示例 :__

示例 1：

输入：arr1 = [1,5,3,6,7], arr2 = [1,3,2,4]
输出：1
解释：用 2 来替换 5，之后 arr1 = [1, 2, 3, 6, 7]。

示例 2：

输入：arr1 = [1,5,3,6,7], arr2 = [4,3,1]
输出：2
解释：用 3 来替换 5，然后用 4 来替换 3，得到 arr1 = [1, 3, 4, 6, 7]。

示例 3：

输入：arr1 = [1,5,3,6,7], arr2 = [1,6,3,3]
输出：-1
解释：无法使 arr1 严格递增。

__提示:__

1 <= arr1.length, arr2.length <= 2000
0 <= arr1[i], arr2[i] <= 10^9

__思路__:

动态规划
定义 dp[i] 表示在 arr1[i] 不替换的情况下 1 - i 递增需要的最少交换次数
在 arr1 的开头和结尾分别插入 -1 和正无穷作为哨兵
将 arr2 去重并排序方便二分查找
如果保留 arr1[i - 1], dp[i] = min(dp[i], dp[i - 1]), 并且此时需要有 arr1[i - 1] < arr1[i]
如果不保留 arr1[i - 1], 二分查找 arr2 中第一个大于或等于 arr1[i] 的数 arr2[j], 然后用 arr2[j - 1] 替换 arr1[i - 1], 然后最多可以替换 k 个, 也就是说保留 arr1[i - k - 1], 将 arr2[j - k:j] 中的元素替换 arr1[i - k:i] 的元素, 此时需要保证 arr1[i - k - 1] < arr2[j - k], arr1 序列才能保持递增, dp[i] = min(dp[i - k - 1] + k), 其中 1 <= k <= min(j, i - 1)
时间复杂度为 O(mn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int makeArrayIncreasing(vector<int>& arr1, vector<int>& arr2) 
    {
        arr1.insert(arr1.begin(), -1);
        arr1.emplace_back(1e9 + 7);
        sort(arr2.begin(), arr2.end());
        arr2.erase(unique(arr2.begin(), arr2.end()), arr2.end());
        vector<int> dp(arr1.size(), 1e9 + 7);
        dp.front() = 0;
        for (int i = 1; i < arr1.size(); i++)
        {
            int j = lower_bound(arr2.begin(), arr2.end(), arr1[i]) - arr2.begin();
            for (int k = 1; k <= min(j, i - 1); k++) if (arr1[i - k - 1] < arr2[j - k]) dp[i] = min(dp[i], dp[i - k - 1] + k);
            if (arr1[i - 1] < arr1[i]) dp[i] = min(dp[i], dp[i - 1]);
        }
        return dp.back() < 1e9 + 7 ? dp.back() : -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int makeArrayIncreasing(int[] arr1, int[] arr2) {
        int n = arr1.length + 2, arr[] = new int[n], dp[] = new int[n];
        arr[0] = -1;
        arr[n - 1] = 1_000_000_007;
        for (int i = 1; i < n - 1; i++) arr[i] = arr1[i - 1];
        TreeSet<Integer> set = new TreeSet<>();
        Arrays.fill(dp, 1_000_000_007);
        dp[0] = 0;
        for (int num : arr2) set.add(num);
        int m = set.size(), idx = 0, arr3[] = new int[m];
        for (int num : set) arr3[idx++] = num;
        for (int i = 1; i < n; i++) {
            int j = ceiling(arr3, arr[i]);
            for (int k = 1; k <= Math.min(j, i - 1); k++) if (arr[i - k - 1] < arr3[j - k]) dp[i] = Math.min(dp[i], dp[i - k - 1] + k);
            if (arr[i - 1] < arr[i]) dp[i] = Math.min(dp[i - 1], dp[i]);
        }
        return dp[n - 1] == 1_000_000_007 ? -1 : dp[n - 1];
    }
    
    private int ceiling(int[] arr, int key) {
        int left = 0, right = arr.length;
        while (left != right) {
            int mid = left + ((right - left) >> 1);
            if (arr[mid] < key) left = mid + 1;
            else right = mid;
        }
        return left;
    }
}
```

__Python__:

```Python
class Solution:
    def makeArrayIncreasing(self, arr1: List[int], arr2: List[int]) -> int:
        arr1, arr2, n = [-1] + arr1 + [float('inf')], sorted(list(set(arr2))), len(arr1) + 2
        dp = [0] + [float('inf')] * (n - 1)
        for i in range(1, n):
            if arr1[i - 1] < arr1[i]:
                dp[i] = min(dp[i - 1], dp[i])
            j = bisect.bisect_left(arr2, arr1[i])
            for k in range(1, min(i - 1, j) + 1):
                if arr1[i - k - 1] < arr2[j - k]:
                    dp[i] = min(dp[i], dp[i - k - 1] + k)
        return dp[-1] if dp[-1] < float('inf') else -1
```
