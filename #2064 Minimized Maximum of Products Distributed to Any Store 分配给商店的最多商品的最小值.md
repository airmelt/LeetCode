# 2064 Minimized Maximum of Products Distributed to Any Store 分配给商店的最多商品的最小值

__Description:__

You are given an integer `n` indicating there are `n` specialty retail stores. There are `m` product types of varying amounts, which are given as a __0-indexed__ integer array `quantities`, where `quantities[i]` represents the number of products of the `i ^ th` product type.

You need to distribute all products to the retail stores following these rules:

- A store can only be given __at most one product type__ but can be given __any__ amount of it.
- After distribution, each store will have been given some number of products (possibly `0`). Let `x` represent the maximum number of products given to any store. You want `x` to be as small as possible, i.e., you want to __minimize__ the __maximum__ number of products that are given to any store.

Return _the minimum possible_ `x`.

__Example:__

Example 1:

```text
Input: n = 6, quantities = [11,6]
Output: 3
Explanation: One optimal way is:
```

- The 11 products of type 0 are distributed to the first four stores in these amounts: 2, 3, 3, 3
- The 6 products of type 1 are distributed to the other two stores in these amounts: 3, 3
The maximum number of products given to any store is max(2, 3, 3, 3, 3, 3) = 3.

Example 2:

```text
Input: n = 7, quantities = [15,10,10]
Output: 5
Explanation: One optimal way is:
```

- The 15 products of type 0 are distributed to the first three stores in these amounts: 5, 5, 5
- The 10 products of type 1 are distributed to the next two stores in these amounts: 5, 5
- The 10 products of type 2 are distributed to the last two stores in these amounts: 5, 5
The maximum number of products given to any store is max(5, 5, 5, 5, 5, 5, 5) = 5.

Example 3:

```text
Input: n = 1, quantities = [100000]
Output: 100000
Explanation: The only optimal way is:
```

- The 100000 products of type 0 are distributed to the only store.

The maximum number of products given to any store is max(100000) = 100000.

__Constraints:__

- `m == quantities.length`
- `1 <= m <= n <= 10 ^ 5`
- `1 <= quantities[i] <= 10 ^ 5`

__题目描述:__

给你一个整数 `n` ，表示有 `n` 间零售商店。总共有 `m` 种产品，每种产品的数目用一个下标从 __0__ 开始的整数数组 `quantities` 表示，其中 `quantities[i]` 表示第 `i` 种商品的数目。

你需要将 所有商品 分配到零售商店，并遵守这些规则：

- 一间商店 __至多__ 只能有 __一种商品__ ，但一间商店拥有的商品数目可以为 __任意__ 件。
- 分配后，每间商店都会被分配一定数目的商品（可能为 `0` 件）。用 `x` 表示所有商店中分配商品数目的最大值，你希望 `x` 越小越好。也就是说，你想 __最小化__ 分配给任意商店商品数目的 __最大值__ 。

请你返回最小的可能的 `x` 。

__示例:__

示例 1：

```text
输入：n = 6, quantities = [11,6]
输出：3
解释： 一种最优方案为：
```

- 11 件种类为 0 的商品被分配到前 4 间商店，分配数目分别为：2，3，3，3 。
- 6 件种类为 1 的商品被分配到另外 2 间商店，分配数目分别为：3，3 。
分配给所有商店的最大商品数目为 max(2, 3, 3, 3, 3, 3) = 3 。

示例 2：

```text
输入：n = 7, quantities = [15,10,10]
输出：5
解释：一种最优方案为：
```

- 15 件种类为 0 的商品被分配到前 3 间商店，分配数目为：5，5，5 。
- 10 件种类为 1 的商品被分配到接下来 2 间商店，数目为：5，5 。
- 10 件种类为 2 的商品被分配到最后 2 间商店，数目为：5，5 。
分配给所有商店的最大商品数目为 max(5, 5, 5, 5, 5, 5, 5) = 5 。

示例 3：

```text
输入：n = 1, quantities = [100000]
输出：100000
解释：唯一一种最优方案为：
```

- 所有 100000 件商品 0 都分配到唯一的商店中。
分配给所有商店的最大商品数目为 max(100000) = 100000 。

__提示：__

- `m == quantities.length`
- `1 <= m <= n <= 10 ^ 5`
- `1 <= quantities[i] <= 10 ^ 5`

__思路:__

```text
二分
查找下界为 1, 上界为最大值 + 1
检查每个商店是否能放下当前的商品向上取整
即对 (q - 1) / x + 1 求和
如果能放下 right = mid
否则 left = mid + 1
直到 left >= right
时间复杂度为 O(NlogM), 空间复杂度为 O(1), M 为数组的最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimizedMaximum(int n, vector<int>& quantities) 
    {
        int left = 1, right = *max_element(quantities.begin(), quantities.end()) + 1;
        while (left < right) 
        {
            int mid = left + ((right - left) >> 1);
            if (check(mid, n, quantities)) right = mid;
            else left = mid + 1;
        }
        return left;
    }
private:
    bool check(int x, int n, vector<int>& quantities) 
    {
        int result = 0;
        for (const auto& q : quantities) result += (q - 1) / x + 1;
        return result <= n;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimizedMaximum(int n, int[] quantities) {
        int left = 1, right = Arrays.stream(quantities).max().getAsInt() + 1;
        while (left < right) {
            int mid = left + ((right - left) >>> 1);
            if (check(mid, n, quantities)) right = mid;
            else left = mid + 1;
        }
        return left;
    }

    private boolean check(int x, int n, int[] quantities) {
        int result = 0;
        for (int q : quantities) result += (q - 1) / x + 1;
        return result <= n;
    }
}
```

__Python__:

```Python
class Solution 
{
public:
    int minimizedMaximum(int n, vector<int>& quantities) 
    {
        int left = 1, right = *max_element(quantities.begin(), quantities.end()) + 1;
        while (left < right) 
        {
            int mid = left + ((right - left) >> 1);
            if (check(mid, n, quantities)) right = mid;
            else left = mid + 1;
        }
        return left;
    }
private:
    bool check(int x, int n, vector<int>& quantities) 
    {
        int result = 0;
        for (const auto& q : quantities) result += (q - 1) / x + 1;
        return result <= n;
    }
};
```
