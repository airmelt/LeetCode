# 1352 Product of the Last K Numbers 最后 K 个数的乘积

__Description:__

Design an algorithm that accepts a stream of integers and retrieves the product of the last k integers of the stream.

Implement the ProductOfNumbers class:

ProductOfNumbers() Initializes the object with an empty stream.
void add(int num) Appends the integer num to the stream.
int getProduct(int k) Returns the product of the last k numbers in the current list. You can assume that always the current list has at least k numbers.
The test cases are generated so that, at any time, the product of any contiguous sequence of numbers will fit into a single 32-bit integer without overflowing.

__Example:__

Input
["ProductOfNumbers","add","add","add","add","add","getProduct","getProduct","getProduct","add","getProduct"]
[[],[3],[0],[2],[5],[4],[2],[3],[4],[8],[2]]

Output
[null,null,null,null,null,null,20,40,0,null,32]

Explanation

```Java
ProductOfNumbers productOfNumbers = new ProductOfNumbers();
productOfNumbers.add(3);        // [3]
productOfNumbers.add(0);        // [3,0]
productOfNumbers.add(2);        // [3,0,2]
productOfNumbers.add(5);        // [3,0,2,5]
productOfNumbers.add(4);        // [3,0,2,5,4]
productOfNumbers.getProduct(2); // return 20. The product of the last 2 numbers is 5 * 4 = 20
productOfNumbers.getProduct(3); // return 40. The product of the last 3 numbers is 2 * 5 * 4 = 40
productOfNumbers.getProduct(4); // return 0. The product of the last 4 numbers is 0 * 2 * 5 * 4 = 0
productOfNumbers.add(8);        // [3,0,2,5,4,8]
productOfNumbers.getProduct(2); // return 32. The product of the last 2 numbers is 4 * 8 = 32 
```

__Constraints:__

0 <= num <= 100
1 <= k <= 4 \* 10^4
At most 4 \* 104 calls will be made to add and getProduct.
The product of the stream at any point in time will fit in a 32-bit integer.

__题目描述：__

请你实现一个「数字乘积类」ProductOfNumbers，要求支持下述两种方法：

1. add(int num)

将数字 num 添加到当前数字列表的最后面。
2. getProduct(int k)

返回当前数字列表中，最后 k 个数字的乘积。
你可以假设当前列表中始终 至少 包含 k 个数字。
题目数据保证：任何时候，任一连续数字序列的乘积都在 32-bit 整数范围内，不会溢出。

__示例：__

输入：
["ProductOfNumbers","add","add","add","add","add","getProduct","getProduct","getProduct","add","getProduct"]
[[],[3],[0],[2],[5],[4],[2],[3],[4],[8],[2]]

输出：
[null,null,null,null,null,null,20,40,0,null,32]

解释：

```Java
ProductOfNumbers productOfNumbers = new ProductOfNumbers();
productOfNumbers.add(3);        // [3]
productOfNumbers.add(0);        // [3,0]
productOfNumbers.add(2);        // [3,0,2]
productOfNumbers.add(5);        // [3,0,2,5]
productOfNumbers.add(4);        // [3,0,2,5,4]
productOfNumbers.getProduct(2); // 返回 20 。最后 2 个数字的乘积是 5 * 4 = 20
productOfNumbers.getProduct(3); // 返回 40 。最后 3 个数字的乘积是 2 * 5 * 4 = 40
productOfNumbers.getProduct(4); // 返回  0 。最后 4 个数字的乘积是 0 * 2 * 5 * 4 = 0
productOfNumbers.add(8);        // [3,0,2,5,4,8]
productOfNumbers.getProduct(2); // 返回 32 。最后 2 个数字的乘积是 4 * 8 = 32 
```

__提示：__

add 和 getProduct 两种操作加起来总共不会超过 40000 次。
0 <= num <= 100
1 <= k <= 40000

__思路：__

前缀积
如果 add 的数为 0, 清空数组
如果数组为空, 先加入哨兵 1
然后加入数组最后一个数和当前数的乘积
返回最后一个数和倒数第 k 个数的商
add 函数时间复杂度为 O(n), getProduct 函数时间复杂度为 O(1), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class ProductOfNumbers 
{
private:
    vector<int> pre;
public:
    ProductOfNumbers() {}
    
    void add(int num) 
    {
        if (!num) 
        {
            pre.clear();
            return;
        }
        if (pre.empty()) pre.emplace_back(1);
        pre.emplace_back(pre.back() * num); 
    }
    
    int getProduct(int k) 
    {
        return k < pre.size() ? pre.back() / pre[pre.size() - 1 - k] : 0;
    }
};

/**
 * Your ProductOfNumbers object will be instantiated and called as such:
 * ProductOfNumbers* obj = new ProductOfNumbers();
 * obj->add(num);
 * int param_2 = obj->getProduct(k);
 */
```

__Java__:

```Java
class ProductOfNumbers {
    private List<Integer> pre;

    public ProductOfNumbers() {
        pre = new ArrayList<>();
    }
    
    public void add(int num) {
        if (num == 0) {
            pre.clear();
            return;
        }
        if (pre.isEmpty()) pre.add(1);
        pre.add(pre.get(pre.size() - 1) * num); 
    }
    
    public int getProduct(int k) {
        return k < pre.size() ? pre.get(pre.size() - 1) / pre.get(pre.size() - 1 - k) : 0;
    }
}

/**
 * Your ProductOfNumbers object will be instantiated and called as such:
 * ProductOfNumbers obj = new ProductOfNumbers();
 * obj.add(num);
 * int param_2 = obj.getProduct(k);
 */
```

__Python__:

```Python
class ProductOfNumbers:

    def __init__(self):
        self.pre = []


    def add(self, num: int) -> None:
        if not num:
            self.pre = []
            return
        if not self.pre:
            self.pre.append(1)
        self.pre.append(self.pre[-1] * num)


    def getProduct(self, k: int) -> int:
        return 0 if k >= len(self.pre) else self.pre[-1] // self.pre[-1 - k]



# Your ProductOfNumbers object will be instantiated and called as such:
# obj = ProductOfNumbers()
# obj.add(num)
# param_2 = obj.getProduct(k)
```
