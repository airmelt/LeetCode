# 2233 Maximum Product After K Increments K 次增加后的最大乘积

__Description:__

You are given an array of non-negative integers `nums` and an integer `k`. In one operation, you may choose __any__ element from `nums` and __increment__ it by `1`.

Return _the __maximum__ __product__ of_ `nums` _after __at most___ `k` _operations._ Since the answer may be very large, return it _modulo_  `10 ^ 9 + 7`. Note that you should maximize the product before taking the modulo.

__Example:__

Example 1:

```text
Input: nums = [0,4], k = 5
Output: 20
Explanation: Increment the first number 5 times.
Now nums = [5, 4], with a product of 5 * 4 = 20.
It can be shown that 20 is maximum product possible, so we return 20.
Note that there may be other ways to increment nums to have the maximum product.
```

Example 2:

```text
Input: nums = [6,3,3,2], k = 2
Output: 216
Explanation: Increment the second number 1 time and increment the fourth number 1 time.
Now nums = [6, 4, 3, 3], with a product of 6 * 4 * 3 * 3 = 216.
It can be shown that 216 is maximum product possible, so we return 216.
Note that there may be other ways to increment nums to have the maximum product.
```

__Constraints:__

- `1 <= nums.length, k <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 6`

__题目描述:__

给你一个非负整数数组 `nums` 和一个整数 `k` 。每次操作，你可以选择 `nums` 中 __任一__ 元素并将它 __增加__ `1` 。

请你返回 __至多__ `k` 次操作后，能得到的 `nums`的 __最大乘积__ 。由于答案可能很大，请你将答案对 `10 ^ 9 + 7` 取余后返回。

__示例:__

示例 1：

```text
输入：nums = [0,4], k = 5
输出：20
解释：将第一个数增加 5 次。
得到 nums = [5, 4] ，乘积为 5 * 4 = 20 。
可以证明 20 是能得到的最大乘积，所以我们返回 20 。
存在其他增加 nums 的方法，也能得到最大乘积。
```

示例 2：

```text
输入：nums = [6,3,3,2], k = 2
输出：216
解释：将第二个数增加 1 次，将第四个数增加 1 次。
得到 nums = [6, 4, 3, 3] ，乘积为 6 * 4 * 3 * 3 = 216 。
可以证明 216 是能得到的最大乘积，所以我们返回 216 。
存在其他增加 nums 的方法，也能得到最大乘积。
```

__提示：__

- `1 <= nums.length, k <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 6`

__思路:__

```text
贪心
给 x 增加 1, 乘积增加 1 + 1 / x
所以每次应该选最小的
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution {
public:
    int maximumProduct(vector<int>& nums, int k) {
        make_heap(nums.begin(),nums.end(),greater<int>());
        while(k--){
            pop_heap(nums.begin(),nums.end(),greater<int>());
            nums.back() += 1;
            push_heap(nums.begin(),nums.end(),greater<int>());
        }
        long long result = 1LL, mod = 1e9 + 7;
        for(int num : nums) result = (result * num) % mod;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumProduct(int[] nums, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for (int num : nums) pq.offer(num);
        while (k-- > 0) pq.offer(pq.poll() + 1);
        long result = 1L, mod = 1_000_000_007L;
        while (!pq.isEmpty()) result = (result * pq.poll()) % mod;
        return (int)result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumProduct(self, nums: List[int], k: int) -> int:
        heapify(nums)
        while (k := k - 1) > -1:
            heapreplace(nums, nums[0] + 1)
        return reduce(lambda x, y: x * y % (10 ** 9 + 7), nums)
```
