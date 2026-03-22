# 638 Shopping Offers 大礼包

__Description__:
In LeetCode Store, there are n items to sell. Each item has a price. However, there are some special offers, and a special offer consists of one or more different kinds of items with a sale price.

You are given an integer array price where price[i] is the price of the ith item, and an integer array needs where needs[i] is the number of pieces of the ith item you want to buy.

You are also given an array special where special[i] is of size n + 1 where special[i][j] is the number of pieces of the jth item in the ith offer and special[i][n] (i.e., the last integer in the array) is the price of the ith offer.

Return the lowest price you have to pay for exactly certain items as given, where you could make optimal use of the special offers. You are not allowed to buy more items than you want, even if that would lower the overall price. You could use any of the special offers as many times as you want.

__Example:__

Example 1:

Input: price = [2,5], special = [[3,0,5],[1,2,10]], needs = [3,2]
Output: 14
Explanation: There are two kinds of items, A and B. Their prices are $2 and $5 respectively.
In special offer 1, you can pay $5 for 3A and 0B
In special offer 2, you can pay $10 for 1A and 2B.
You need to buy 3A and 2B, so you may pay $10 for 1A and 2B (special offer #2), and $4 for 2A.

Example 2:

Input: price = [2,3,4], special = [[1,1,0,4],[2,2,1,9]], needs = [1,2,1]
Output: 11
Explanation: The price of A is $2, and $3 for B, $4 for C.
You may pay $4 for 1A and 1B, and $9 for 2A ,2B and 1C.
You need to buy 1A ,2B and 1C, so you may pay $4 for 1A and 1B (special offer #1), and $3 for 1B, $4 for 1C.
You cannot add more items, though only $9 for 2A ,2B and 1C.

__Constraints:__

n == price.length
n == needs.length
1 <= n <= 6
0 <= price[i] <= 10
0 <= needs[i] <= 10
1 <= special.length <= 100
special[i].length == n + 1
0 <= special[i][j] <= 50

__题目描述__:
在 LeetCode 商店中， 有 n 件在售的物品。每件物品都有对应的价格。然而，也有一些大礼包，每个大礼包以优惠的价格捆绑销售一组物品。

给你一个整数数组 price 表示物品价格，其中 price[i] 是第 i 件物品的价格。另有一个整数数组 needs 表示购物清单，其中 needs[i] 是需要购买第 i 件物品的数量。

还有一个数组 special 表示大礼包，special[i] 的长度为 n + 1 ，其中 special[i][j] 表示第 i 个大礼包中内含第 j 件物品的数量，且 special[i][n] （也就是数组中的最后一个整数）为第 i 个大礼包的价格。

返回 确切 满足购物清单所需花费的最低价格，你可以充分利用大礼包的优惠活动。你不能购买超出购物清单指定数量的物品，即使那样会降低整体价格。任意大礼包可无限次购买。

__示例 :__

示例 1：

输入：price = [2,5], special = [[3,0,5],[1,2,10]], needs = [3,2]
输出：14
解释：有 A 和 B 两种物品，价格分别为 ¥2 和 ¥5 。
大礼包 1 ，你可以以 ¥5 的价格购买 3A 和 0B 。
大礼包 2 ，你可以以 ¥10 的价格购买 1A 和 2B 。
需要购买 3 个 A 和 2 个 B ， 所以付 ¥10 购买 1A 和 2B（大礼包 2），以及 ¥4 购买 2A 。

示例 2：

输入：price = [2,3,4], special = [[1,1,0,4],[2,2,1,9]], needs = [1,2,1]
输出：11
解释：A ，B ，C 的价格分别为 ¥2 ，¥3 ，¥4 。
可以用 ¥4 购买 1A 和 1B ，也可以用 ¥9 购买 2A ，2B 和 1C 。
需要买 1A ，2B 和 1C ，所以付 ¥4 买 1A 和 1B（大礼包 1），以及 ¥3 购买 1B ， ¥4 购买 1C 。
不可以购买超出待购清单的物品，尽管购买大礼包 2 更加便宜。

__提示:__

n == price.length
n == needs.length
1 <= n <= 6
0 <= price[i] <= 10
0 <= needs[i] <= 10
1 <= special.length <= 100
special[i].length == n + 1
0 <= special[i][j] <= 50

__思路__:

回溯 ➕ 剪枝
对每一个大礼包进行回溯, 得到的钱大于全局最小值的就直接丢弃
最后再计算直接购买的价格
时间复杂度 O(2 ^ n), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int shoppingOffers(vector<int>& price, vector<vector<int>>& special, vector<int>& needs) 
    {
        result = direct(price, needs);
        helper(price, special, needs, 0, 0);
        return result;
    }
private:
    int result = 0;
    
    int direct(vector<int>& price, vector<int>& needs) 
    {
        int total = 0, n = price.size();;
        for (int i = 0; i < n; i++) total += price[i] * needs[i];
        return total;
    }
    
    bool valid(vector<int>& offer, vector<int>& needs) 
    {
        int n = needs.size();
        for (int i = 0; i < n; i++) if (offer[i] > needs[i]) return false;
        return true;
    }
    
    void helper(vector<int>& price, vector<vector<int>>& special, vector<int>& needs, int index, int total) 
    {
        if (total >= result) return;
        if (index == special.size()) 
        {
            total += direct(price, needs);
            result = min(result, total);
            return;
        }
        vector<int> offer = special[index];
        int m = needs.size();
        if (valid(offer, needs)) 
        {
            vector<int> cur;
            for (int i = 0; i < m; i++) cur.emplace_back(needs[i] - offer[i]);
            helper(price, special, cur, index, total + offer.back());
        }
        helper(price, special, needs, index + 1, total);
    }
};
```

__Java__:

```Java
class Solution {
    private int result = 0;
    public int shoppingOffers(List<Integer> price, List<List<Integer>> special, List<Integer> needs) {
        result = direct(price, needs);
        helper(price, special, needs, 0, 0);
        return result;
    }
    
    private int direct(List<Integer> price, List<Integer> needs) {
        int total = 0, n = price.size();;
        for (int i = 0; i < n; i++) total += price.get(i) * needs.get(i);
        return total;
    }
    
    private boolean valid(List<Integer> offer, List<Integer> needs) {
        int n = needs.size();
        for (int i = 0; i < n; i++) if (offer.get(i) > needs.get(i)) return false;
        return true;
    }
    
    private void helper(List<Integer> price, List<List<Integer>> special, List<Integer> needs, int index, int total) {
        if (total >= result) return;
        if (index == special.size()) {
            total += direct(price, needs);
            result = Math.min(result, total);
            return;
        }
        List<Integer> offer = special.get(index);
        int m = needs.size();
        if (valid(offer, needs)) {
            List<Integer> cur = new ArrayList<>();
            for (int i = 0; i < m; i++) cur.add(needs.get(i) - offer.get(i));
            helper(price, special, cur, index, total + offer.get(m));
        }
        helper(price, special, needs, index + 1, total);
    }
}
```

__Python__:

```Python
class Solution:
    def shoppingOffers(self, price: List[int], special: List[List[int]], needs: List[int]) -> int:
        n = len(price)
        special = list(filter(lambda x: x[-1] < sum(x[i] * price[i] for i in range(n)), special))
        def helper(cur: List[int], targets: List[int]) -> int:
            if not sum(targets):
                return 0
            cur = list(filter(lambda x: all(x[i] <= targets[i] for i in range(n)), cur))  
            if not cur:
                return sum(targets[i] * price[i] for i in range(n))
            result = []
            for pack in cur:
                result.append(pack[-1] + helper(cur, [targets[i] - pack[i] for i in range(n)]))
            return min(result)
        return helper(special, needs)
```
