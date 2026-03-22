# 327 Count of Range Sum 区间和的个数

__Description__:
Given an integer array nums, return the number of range sums that lie in [lower, upper] inclusive.
Range sum S(i, j) is defined as the sum of the elements in nums between indices i and j (i ≤ j), inclusive.

__Note:__
A naive algorithm of O(n2) is trivial. You MUST do better than that.

__Example:__

Input: nums = [-2,5,-1], lower = -2, upper = 2,
Output: 3
Explanation: The three ranges are : [0,0], [2,2], [0,2] and their respective sums are: -2, -1, 2.

__Constraints:__

0 <= nums.length <= 10^4

__题目描述__:
给定一个整数数组 nums，返回区间和在 [lower, upper] 之间的个数，包含 lower 和 upper。
区间和 S(i, j) 表示在 nums 中，位置从 i 到 j 的元素之和，包含 i 和 j (i ≤ j)。

__说明:__
最直观的算法复杂度是 O(n2) ，请在此基础上优化你的算法。

__示例 :__

输入: nums = [-2,5,-1], lower = -2, upper = 2,
输出: 3
解释: 3个区间分别是: [0,0], [2,2], [0,2]，它们表示的和分别为: -2, -1, 2。

__思路__:

1. 暴力法
利用前缀和
遍历前缀和数组
只要对应前缀和数组区间[i, j + 1], 求出区间和落在 [lower, upper]范围的即可
时间复杂度O(n ^ 2), 空间复杂度O(n)
2. 二分查找
在计算前缀和时, 使得前缀和保持有序
时间复杂度O(n ^ 2), 空间复杂度O(n)
3. 归并排序
由 lower <= sum[j] - sum[i] <= upper
在每一次加入新的区间和的时候使用归并排序, 就能同时得到区间
时间复杂度O(nlgn), 空间复杂度O(n)
4. 树状数组
每次加入一个新的数就更新
时间复杂度O(nlgn), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int countRangeSum(vector<int>& nums, int lower, int upper) 
    {
        vector<long> v = {0};
        for (int i = 0; i < nums.size(); ++i) v.emplace_back(v[i] + nums[i]);     
        return merge_sort(v, 0, v.size(), lower, upper);
    }
private:
    int merge_sort(vector<long>& nums, int l, int h, int lower, int upper) 
    {
        if (h - l <= 1) return 0;
        int mid = l + (h - l >> 1);
        int count = merge_sort(nums, l, mid, lower, upper) + merge_sort(nums, mid, h, lower, upper);
        int right1 = mid, right2 = mid;
        for (int left = l; left < mid; ++left) 
        {
            while (right1 != h and nums[right1] - nums[left] < lower) ++right1;
            while (right2 != h and nums[right2] - nums[left] <= upper) ++right2;
            count += right2 - right1;
        }
        inplace_merge(nums.begin() + l, nums.begin() + mid, nums.begin() + h);
        return count;
    }
};
```

__Java__:

```Java
class Solution {
    private int N;
    private int[] tree;
    public int countRangeSum(int[] nums, int lower, int upper) {
        List<Long> list = new ArrayList();
        list.add(0L);
        long sum = 0, n = nums.length;
        for (int i = 0; i < n; i++) {
            sum += nums[i];
            list.add(sum); 
            list.add(sum - upper); 
            list.add(sum - lower);
        }
        Collections.sort(list);
        list = unique(list);
        N = list.size();
        tree = new int[N + 1];   //这里树状数组的下标从1开始
        add(binaryFind(list, 0) + 1, 1);  //加入前缀和为0的情况
        sum = 0;
        int ans = 0;
        for(int i = 0; i < n; i++){
            sum += nums[i];
            int left = binaryFind(list, sum - upper) + 1;
            int right = binaryFind(list, sum - lower) + 1;
            ans += query(right) - query(left - 1);
            add(binaryFind(list, sum) + 1, 1);
        }
        return ans;
    }

    public int binaryFind(List<Long> list, long target){
        int l = 0, r = list.size() - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (list.get(mid) >= target) r = mid;
            else l = mid + 1;
        }
        return l;
    }

    public void add(int x, int c) {
        for (int i = x; i <= N; i += lowBit(i)) tree[i] += c;
    }

    public int query(int x) {
        int result = 0;
        for (int i = x; i > 0; i -= lowBit(i)) result += tree[i];
        return result;
    }

    public int lowBit(int x){
        return x & -x;
    }


    public List<Long> unique(List<Long> list){
        List<Long> result = new ArrayList();
        for (int i = 0; i < list.size(); i++) if (result.isEmpty() || result.get(result.size() - 1) != list.get(i)) result.add(list.get(i));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countRangeSum(self, nums: List[int], lower: int, upper: int) -> int:
        result, cur, s = 0, 0, [0]
        for num in nums:
            cur += num
            result += bisect.bisect_right(s, cur - lower) - bisect.bisect_left(s, cur - upper)
            bisect.insort_right(s, cur)
        return result
```
