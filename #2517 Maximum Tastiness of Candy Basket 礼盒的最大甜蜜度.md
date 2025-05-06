# 2517 Maximum Tastiness of Candy Basket 礼盒的最大甜蜜度

__Description:__

You are given an array of positive integers `price` where `price[i]` denotes the price of the `i ^ th` candy and a positive integer `k`.

The store sells baskets of `k` __distinct__ candies. The __tastiness__ of a candy basket is the smallest absolute difference of the __prices__ of any two candies in the basket.

Return the maximum tastiness of a candy basket.

__Example:__

Example 1:

```text
Input: price = [13,5,1,8,21,2], k = 3
Output: 8
Explanation: Choose the candies with the prices [13,5,21].
The tastiness of the candy basket is: min(|13 - 5|, |13 - 21|, |5 - 21|) = min(8, 8, 16) = 8.
It can be proven that 8 is the maximum tastiness that can be achieved.
```

Example 2:

```text
Input: price = [1,3,1], k = 2
Output: 2
Explanation: Choose the candies with the prices [1,3].
The tastiness of the candy basket is: min(|1 - 3|) = min(2) = 2.
It can be proven that 2 is the maximum tastiness that can be achieved.
```

Example 3:

```text
Input: price = [7,7,7,7], k = 2
Output: 0
Explanation: Choosing any two distinct candies from the candies we have will result in a tastiness of 0.
```

__Constraints:__

- `2 <= k <= price.length <= 10 ^ 5`
- `1 <= price[i] <= 10 ^ 9`

__题目描述:__

给你一个正整数数组 `price` ，其中 `price[i]` 表示第 `i` 类糖果的价格，另给你一个正整数 `k` 。

商店组合 `k` 类 __不同__ 糖果打包成礼盒出售。礼盒的 __甜蜜度__ 是礼盒中任意两种糖果 __价格__ 绝对差的最小值。

返回礼盒的 最大 甜蜜度。

__示例:__

示例 1：

```text
输入：price = [13,5,1,8,21,2], k = 3
输出：8
解释：选出价格分别为 [13,5,21] 的三类糖果。
礼盒的甜蜜度为 min(|13 - 5|, |13 - 21|, |5 - 21|) = min(8, 8, 16) = 8 。
可以证明能够取得的最大甜蜜度就是 8 。
```

示例 2：

```text
输入：price = [1,3,1], k = 2
输出：2
解释：选出价格分别为 [1,3] 的两类糖果。 
礼盒的甜蜜度为 min(|1 - 3|) = min(2) = 2 。
可以证明能够取得的最大甜蜜度就是 2 。
```

示例 3：

```text
输入：price = [7,7,7,7], k = 2
输出：0
解释：从现有的糖果中任选两类糖果，甜蜜度都会是 0 。
```

__提示：__

- `2 <= k <= price.length <= 10 ^ 5`
- `1 <= price[i] <= 10 ^ 9`

__思路:__

```text
二分 ➕ 贪心
先将 price 排序
左边界为 0, 右边界为 (price[n - 1] - price[0]) / (k - 1), 左开右闭
因为 d 为间隔, 取 k 个 pre[0] + (k - 1) * d <= price[n - 1]
所以 d <= (price[n - 1] - price[0]) / (k - 1)
每次选择 mid 作为 d
从第一个开始选, 找到下一个大于等于 pre + mid 的数就可以选
如果选到 k 个, 说明 d 太小了, left = mid + 1
否则 d 太大了, right = mid - 1
返回 right
时间复杂度为 O(NlogN + NlogU), 空间复杂度为 O(logN), U 为 price 的范围, 本题为 10 ^ 9
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumTastiness(vector<int>& price, int k) 
    {
        sort(price.begin(), price.end());
        int left = 0, right = (price.back() - price.front()) / (k - 1);
        while (left <= right)
        {
            int mid = left + ((right - left) >> 1), cur = 1, pre = price.front();
            for (const auto& p : price)
            {
                if (p >= pre + mid)
                {
                    ++cur;
                    pre = p;
                }
            }
            if (cur >= k) left = mid + 1;
            else right = mid - 1;
        }    
        return right;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumTastiness(int[] price, int k) {
        Arrays.sort(price);
        int n = price.length, left = 0, right = (price[n - 1] - price[0]) / (k - 1);
        while (left <= right) {
            int mid = left + ((right - left) >>> 1);
            if (check(price, mid) >= k) left = mid + 1;
            else right = mid - 1;
        }
        return right;
    }

    private int check(int[] price, int mid) {
        int cur = 1, pre = price[0];
        for (int p : price) {
            if (p >= pre + mid) {
                ++cur;
                pre = p;
            }
        }
        return cur;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumTastiness(self, price: List[int], k: int) -> int:
        def check(d: int) -> bool:
            cur, pre = 1, price[0]
            for p in price:
                if p >= pre + d + 1:
                    cur += 1
                    pre = p
            return cur < k
        price.sort()
        return bisect_left(range((price[-1] - price[0]) // (k - 1)), True, key=check)
```
