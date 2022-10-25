# 1357 Apply Discount Every n Orders 每隔 n 个顾客打折

__Description:__

There is a supermarket that is frequented by many customers. The products sold at the supermarket are represented as two parallel integer arrays products and prices, where the ith product has an ID of products[i] and a price of prices[i].

When a customer is paying, their bill is represented as two parallel integer arrays product and amount, where the jth product they purchased has an ID of product[j], and amount[j] is how much of the product they bought. Their subtotal is calculated as the sum of each amount[j] * (price of the jth product).

The supermarket decided to have a sale. Every nth customer paying for their groceries will be given a percentage discount. The discount amount is given by discount, where they will be given discount percent off their subtotal. More formally, if their subtotal is bill, then they would actually pay bill * ((100 - discount) / 100).

Implement the Cashier class:

Cashier(int n, int discount, int[] products, int[] prices) Initializes the object with n, the discount, and the products and their prices.
double getBill(int[] product, int[] amount) Returns the final total of the bill with the discount applied (if any). Answers within 10-5 of the actual value will be accepted.

__Example:__

Example 1:

Input
["Cashier","getBill","getBill","getBill","getBill","getBill","getBill","getBill"]
[[3,50,[1,2,3,4,5,6,7],[100,200,300,400,300,200,100]],[[1,2],[1,2]],[[3,7],[10,10]],[[1,2,3,4,5,6,7],[1,1,1,1,1,1,1]],[[4],[10]],[[7,3],[10,10]],[[7,5,3,1,6,4,2],[10,10,10,9,9,9,7]],[[2,3,5],[5,3,2]]]
Output
[null,500.0,4000.0,800.0,4000.0,4000.0,7350.0,2500.0]
Explanation

```Java
Cashier cashier = new Cashier(3,50,[1,2,3,4,5,6,7],[100,200,300,400,300,200,100]);
cashier.getBill([1,2],[1,2]);                        // return 500.0. 1st customer, no discount.
                                                     // bill = 1 * 100 + 2 * 200 = 500.
cashier.getBill([3,7],[10,10]);                      // return 4000.0. 2nd customer, no discount.
                                                     // bill = 10 * 300 + 10 * 100 = 4000.
cashier.getBill([1,2,3,4,5,6,7],[1,1,1,1,1,1,1]);    // return 800.0. 3rd customer, 50% discount.
                                                     // Original bill = 1600
                                                     // Actual bill = 1600 * ((100 - 50) / 100) = 800.
cashier.getBill([4],[10]);                           // return 4000.0. 4th customer, no discount.
cashier.getBill([7,3],[10,10]);                      // return 4000.0. 5th customer, no discount.
cashier.getBill([7,5,3,1,6,4,2],[10,10,10,9,9,9,7]); // return 7350.0. 6th customer, 50% discount.
                                                     // Original bill = 14700, but with
                                                     // Actual bill = 14700 * ((100 - 50) / 100) = 7350.
cashier.getBill([2,3,5],[5,3,2]);                    // return 2500.0.  6th customer, no discount.
```

__Constraints:__

1 <= n <= 10^4
0 <= discount <= 100
1 <= products.length <= 200
prices.length == products.length
1 <= products[i] <= 200
1 <= prices[i] <= 1000
The elements in products are unique.
1 <= product.length <= products.length
amount.length == product.length
product[j] exists in products.
1 <= amount[j] <= 1000
The elements of product are unique.
At most 1000 calls will be made to getBill.
Answers within 10^-5 of the actual value will be accepted.

__题目描述：__

超市里正在举行打折活动，每隔 n 个顾客会得到 discount 的折扣。

超市里有一些商品，第 i 种商品为 products[i] 且每件单品的价格为 prices[i] 。

结账系统会统计顾客的数目，每隔 n 个顾客结账时，该顾客的账单都会打折，折扣为 discount （也就是如果原本账单为 x ，那么实际金额会变成 x - (discount * x) / 100 ），然后系统会重新开始计数。

顾客会购买一些商品， product[i] 是顾客购买的第 i 种商品， amount[i] 是对应的购买该种商品的数目。

请你实现 Cashier 类：

Cashier(int n, int discount, int[] products, int[] prices) 初始化实例对象，参数分别为打折频率 n ，折扣大小 discount ，超市里的商品列表 products 和它们的价格 prices 。
double getBill(int[] product, int[] amount) 返回账单的实际金额（如果有打折，请返回打折后的结果）。返回结果与标准答案误差在 10^-5 以内都视为正确结果。

__示例：__

示例 1：

输入
["Cashier","getBill","getBill","getBill","getBill","getBill","getBill","getBill"]
[[3,50,[1,2,3,4,5,6,7],[100,200,300,400,300,200,100]],[[1,2],[1,2]],[[3,7],[10,10]],[[1,2,3,4,5,6,7],[1,1,1,1,1,1,1]],[[4],[10]],[[7,3],[10,10]],[[7,5,3,1,6,4,2],[10,10,10,9,9,9,7]],[[2,3,5],[5,3,2]]]
输出
[null,500.0,4000.0,800.0,4000.0,4000.0,7350.0,2500.0]
解释

```Java
Cashier cashier = new Cashier(3,50,[1,2,3,4,5,6,7],[100,200,300,400,300,200,100]);
cashier.getBill([1,2],[1,2]);                        // 返回 500.0, 账单金额为 = 1 * 100 + 2 * 200 = 500.
cashier.getBill([3,7],[10,10]);                      // 返回 4000.0
cashier.getBill([1,2,3,4,5,6,7],[1,1,1,1,1,1,1]);    // 返回 800.0 ，账单原本为 1600.0 ，但由于该顾客是第三位顾客，他将得到 50% 的折扣，所以实际金额为 1600 - 1600 * (50 / 100) = 800 。
cashier.getBill([4],[10]);                           // 返回 4000.0
cashier.getBill([7,3],[10,10]);                      // 返回 4000.0
cashier.getBill([7,5,3,1,6,4,2],[10,10,10,9,9,9,7]); // 返回 7350.0 ，账单原本为 14700.0 ，但由于系统计数再次达到三，该顾客将得到 50% 的折扣，实际金额为 7350.0 。
cashier.getBill([2,3,5],[5,3,2]);                    // 返回 2500.0
```

__提示：__

1 <= n <= 10^4
0 <= discount <= 100
1 <= products.length <= 200
1 <= products[i] <= 200
products 列表中 不会 有重复的元素。
prices.length == products.length
1 <= prices[i] <= 1000
1 <= product.length <= products.length
product[i] 在 products 出现过。
amount.length == product.length
1 <= amount[i] <= 1000
最多有 1000 次对 getBill 函数的调用。
返回结果与标准答案误差在 10^-5 以内都视为正确结果。

__思路：__

模拟
先用哈希表存放下所有商品的价格
每次计算 bill 的时候用一个全局计数器统计, 自增到 n 时置 0 或者使用取余计算并给予折扣
预处理时间复杂度为 O(n), getBill 函数时间复杂度为 O(m), 空间复杂度为 O(n), 其中 n 表示 products/prices 数组的长度, m 表示 product/amount 数组的长度

__代码__:

__C++__:

```C++
class Cashier 
{
private:
    int n, discount, count;
    unordered_map<int, int> m;
public:
    Cashier(int _n, int _discount, vector<int>& products, vector<int>& prices) : n(_n), discount(_discount), count(0)
    {
        for (int i = 0, s = products.size(); i < s; i++) m[products[i]] = prices[i];
    }
    
    double getBill(vector<int> product, vector<int> amount) 
    {
        ++count;
        double result = 0;
        for (int i = 0, s = product.size(); i < s; i++) result += m[product[i]] * amount[i];
        return result - (!(count % n) ? (discount * result) / 100 : 0);
    }
};

/**
 * Your Cashier object will be instantiated and called as such:
 * Cashier* obj = new Cashier(n, discount, products, prices);
 * double param_1 = obj->getBill(product,amount);
 */
```

__Java__:

```Java
class Cashier {
    private int n, discount, count;
    private Map<Integer, Integer> map = new HashMap<>();

    public Cashier(int n, int discount, int[] products, int[] prices) {
        this.n = n;
        this.discount = discount;
        for (int i = 0, m = products.length; i < m; i++) map.put(products[i], prices[i]);
    }
    
    public double getBill(int[] product, int[] amount) {
        ++count;
        double result = 0;
        for (int i = 0, m = product.length; i < m; i++) result += map.get(product[i]) * amount[i];
        return result - (count % n == 0 ? (discount * result) / 100 : 0);
    }
}

/**
 * Your Cashier object will be instantiated and called as such:
 * Cashier obj = new Cashier(n, discount, products, prices);
 * double param_1 = obj.getBill(product,amount);
 */
```

__Python__:

```Python
class Cashier:

    def __init__(self, n: int, discount: int, products: List[int], prices: List[int]):
        self.n = n
        self.discount = discount
        self.count = 0
        self.prices = {product: prices[i] for i, product in enumerate(products)}


    def getBill(self, product: List[int], amount: List[int]) -> float:
        result = 0
        for i, p in enumerate(product):
            result += self.prices[p] * amount[i]
        self.count += 1
        return result - (self.discount * result / 100.0 if not self.count % self.n else 0)



# Your Cashier object will be instantiated and called as such:
# obj = Cashier(n, discount, products, prices)
# param_1 = obj.getBill(product,amount)
```
