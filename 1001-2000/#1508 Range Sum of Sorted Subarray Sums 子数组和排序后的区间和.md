# 1508 Range Sum of Sorted Subarray Sums 子数组和排序后的区间和

__Description:__

You are given the array `nums` consisting of `n` positive integers. You computed the sum of all non-empty continuous subarrays from the array and then sorted them in non-decreasing order, creating a new array of `n * (n + 1) / 2` numbers.

_Return the sum of the numbers from index_ `left` _to index_ `right` (__indexed from 1__)_, inclusive, in the new array._ Since the answer can be a huge number return it modulo `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4], n = 4, left = 1, right = 5
Output: 13 
Explanation: All subarray sums are 1, 3, 6, 10, 2, 5, 9, 3, 7, 4. After sorting them in non-decreasing order we have the new array [1, 2, 3, 3, 4, 5, 6, 7, 9, 10]. The sum of the numbers from index le = 1 to ri = 5 is 1 + 2 + 3 + 3 + 4 = 13.
```

Example 2:

```text
Input: nums = [1,2,3,4], n = 4, left = 3, right = 4
Output: 6
Explanation: The given array is the same as example 1. We have the new array [1, 2, 3, 3, 4, 5, 6, 7, 9, 10]. The sum of the numbers from index le = 3 to ri = 4 is 3 + 3 = 6.
```

Example 3:

```text
Input: nums = [1,2,3,4], n = 4, left = 1, right = 10
Output: 50
```

__Constraints:__

- `n == nums.length`
- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 100`
- `1 <= left <= right <= n * (n + 1) / 2`

__题目描述:__

给你一个数组 `nums` ，它包含 `n` 个正整数。你需要计算所有非空连续子数组的和，并将它们按升序排序，得到一个新的包含 `n * (n + 1) / 2` 个数字的数组。

请你返回在新数组中下标为 `left` 到 `right` __（下标从 1 开始）__ 的所有数字和（包括左右端点）。由于答案可能很大，请你将它对 10 ^ 9 + 7 取模后返回。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4], n = 4, left = 1, right = 5
输出：13 
解释：所有的子数组和为 1, 3, 6, 10, 2, 5, 9, 3, 7, 4 。将它们升序排序后，我们得到新的数组 [1, 2, 3, 3, 4, 5, 6, 7, 9, 10] 。下标从 le = 1 到 ri = 5 的和为 1 + 2 + 3 + 3 + 4 = 13 。
```

示例 2：

输入：nums = [1,2,3,4], n = 4, left = 3, right = 4
输出：6
解释：给定数组与示例 1 一样，所以新数组为 [1, 2, 3, 3, 4, 5, 6, 7, 9, 10] 。下标从 le = 3 到 ri = 4 的和为 3 + 3 = 6 。

示例 3：

```text
输入：nums = [1,2,3,4], n = 4, left = 1, right = 10
输出：50
```

__提示：__

- `1 <= nums.length <= 10 ^ 3`
- `nums.length == n`
- `1 <= nums[i] <= 100`
- `1 <= left <= right <= n * (n + 1) / 2`

__思路:__

```text
1. 暴力法
因为数组范围仅为 10 ^ 3
可以尝试使用暴力法
将所有非空连续子数组的值计算出来
然后排序计算子数组范围的和
时间复杂度为 O(N ^ 2logN), 空间复杂度为 O(N ^ 2)
2. 优先队列 ➕ 多路归并
将 nums 排序并看作 n 条链表
每次选择最小的链表头加入新的值到优先队列中
比如 [1, 2, 3, 4]
第一条链表为 1 -> 3 -> 6 -> 10
优先队列可以同时完成排序
记录下每个值的下标每次选择下标 ➕ 1 加入优先队列即可
时间复杂度为 O(N ^ 2logN), 空间复杂度为 O(N), 时间复杂度为 O((R - L)logN), 但是 O(R - L) 实际上就为 O(N ^ 2), 优先队列中最多只有 N 项
3. 二分查找 ➕ 前缀和
以一个数组 [1, 2, 3, 4] 为例
假定子数组的和的数组为 arr = [1, 2, 3, 3, 4, 5, 6, 7, 9, 10]
前缀和 presum = [0, 1, 3, 6, 10]
要求 arr 第 k 个数可以通过 presum 来求取
比如说求 arr[-2] = presum[-1] - presum[1]
或者 arr[2] = presum[2] - presum[1]
可在 O(1) 的时间内求得
使用二分搜索搜索 [0, presum[-1]], 可以在 O(log(sum(nums))) 得到 arr[k]
第 1 层为 [0, 1, 3, 6, 10]
第 2 层可以用每个元素减去 nums[0] 得到 [0, 0, 2, 5, 9], 以此类推
记 get_count(x) 为 arr[k] = x 的下标
比如说 arr[4] = 4, 第 1 层可以找到 3 对应下标为 2 也就是说有 2 个满足
接下来的每层都比使用同样的方式, 每一层的个数为 j - i - 1, 注意到每一层都是由上一层减去第一个数得到的, 所以在移动到下一层时 j 指针只需要往前移动 1 个(这个矩阵从上往下是递减的)
在得到小于等于 x 的下标为 k 之后
我们就需要从 arr 的前缀和中找到 prearr[right] - prearr[left - 1]
即求 prearr[k]
比如要求第 2 层 [2, 5] 这两个元素对应的 arr 的值
实际上它的前缀和为 [3, 6], 因为它是由第 1 层减去 1 得到的
所以应该用 presum_j[j] - presum_j[i] - (j - i) * presum[i] 来计算
这里 j 表示层数, i 表示该层结点的下标
注意到 presum 的前缀和 prepresum [0, 1, 4, 10, 20]
可以将上式改写为 prepresum[j] - prepresum[i] - (j - i) * presum[i]
由于只使用了数组的和与数组的顺序无关, 所以不需要排序
时间复杂度为 O(NlogS), 空间复杂度为 O(N), 其中 S = sum(nums)
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    static constexpr int mod = 1e9 + 7;

    int get_sum(vector<int>& presum, vector<int> &prepresum, int n, int k) 
    {
        int num = get_kth(presum, n, k), sum = 0, count = 0;
        for (int i = 0, j = 1; i < n; i++) 
        {
            while (j <= n and presum[j] - presum[i] < num) ++j;
            sum += prepresum[--j] - prepresum[i] - presum[i] * (j - i);
            sum %= mod;
            count += j - i;
        }
        sum += num * (k - count);
        return sum;
    }

    int get_kth(vector<int> &presum, int n, int k) 
    {
        int low = 0, high = presum[n];
        while (low < high) 
        {
            int mid = low + ((high - low) >> 1);
            if (get_count(presum, n, mid) < k) low = mid + 1;
            else high = mid;
        }
        return low;
    }
    
    int get_count(vector<int>& presum, int n, int x) 
    {
        int count = 0;
        for (int i = 0, j = 1; i < n; i++, j--) 
        {
            while (j <= n and presum[j] - presum[i] <= x) ++j;
            count += j - i - 1;
        }
        return count;
    }
public:
    int rangeSum(vector<int>& nums, int n, int left, int right) 
    {
        vector<int> presum(n + 1), prepresum(n + 1);
        for (int i = 0; i < n; i++) presum[i + 1] = presum[i] + nums[i];
        for (int i = 0; i < n; i++) prepresum[i + 1] = prepresum[i] + presum[i + 1];
        return (get_sum(presum, prepresum, n, right) - get_sum(presum, prepresum, n, left - 1)) % mod;
    }
};
```

__Java__:

```Java
class Solution {
    public int rangeSum(int[] nums, int n, int left, int right) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for (int i = 0; i < n; i++) pq.offer(nums[i] * 10000 + i);
        int cnt = 0, MOD = 1_000_000_007;
        long result = 0;
        while (++cnt <= right) {
            int cur = pq.poll(), idx = cur % 10000, value = cur / 10000;
            if (cnt >= left) result = (result + value) % MOD;
            if (idx < n - 1) pq.offer((value + nums[idx + 1]) * 10000 + idx + 1);
        }
        return (int)result;
    }
}
```

__Python__:

```Python
class Solution:
    def rangeSum(self, nums: List[int], n: int, left: int, right: int) -> int:
        return sum(sorted(sum(nums[i:j]) for i in range(len(nums)) for j in range(i + 1, len(nums) + 1))[left - 1:right]) % (10 ** 9 + 7)
```
