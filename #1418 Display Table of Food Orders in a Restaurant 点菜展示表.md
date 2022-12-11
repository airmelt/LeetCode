# 1418 Display Table of Food Orders in a Restaurant 点菜展示表

__Description:__

Given the array `orders`, which represents the orders that customers have done in a restaurant. More specifically `orders[i]=[customerNamei,tableNumberi,foodItemi]` where `customerNamei` is the name of the customer, `tableNumberi` is the table customer sit at, and `foodItemi` is the item customer orders.

Return the restaurant's “display table”. The “display table” is a table whose row entries denote how many of each food item each table ordered. The first column is the table number and the remaining columns correspond to each food item in alphabetical order. The first row should be a header whose first column is “Table”, followed by the names of the food items. Note that the customer names are not part of the table. Additionally, the rows should be sorted in numerically increasing order.

__Example:__

Example 1:

```text
Input: orders = [["David","3","Ceviche"],["Corina","10","Beef Burrito"],["David","3","Fried Chicken"],["Carla","5","Water"],["Carla","5","Ceviche"],["Rous","3","Ceviche"]]
Output: [["Table","Beef Burrito","Ceviche","Fried Chicken","Water"],["3","0","2","1","0"],["5","0","1","0","1"],["10","1","0","0","0"]] 
Explanation:
The displaying table looks like:
Table,Beef Burrito,Ceviche,Fried Chicken,Water
3    ,0           ,2      ,1            ,0
5    ,0           ,1      ,0            ,1
10   ,1           ,0      ,0            ,0
For the table 3: David orders "Ceviche" and "Fried Chicken", and Rous orders "Ceviche".
For the table 5: Carla orders "Water" and "Ceviche".
For the table 10: Corina orders "Beef Burrito".
```

Example 2:

```text
Input: orders = [["James","12","Fried Chicken"],["Ratesh","12","Fried Chicken"],["Amadeus","12","Fried Chicken"],["Adam","1","Canadian Waffles"],["Brianna","1","Canadian Waffles"]]
Output: [["Table","Canadian Waffles","Fried Chicken"],["1","2","0"],["12","0","3"]] 
Explanation: 
For the table 1: Adam and Brianna order "Canadian Waffles".
For the table 12: James, Ratesh and Amadeus order "Fried Chicken".
```

Example 3:

```text
Input: orders = [["Laura","2","Bean Burrito"],["Jhon","2","Beef Burrito"],["Melissa","2","Soda"]]
Output: [["Table","Bean Burrito","Beef Burrito","Soda"],["2","1","1","1"]]
```

__Constraints:__

- `1 <= orders.length <= 5 * 10 ^ 4`
- `orders[i].length == 3`
- `1 <= customerNamei.length, foodItemi.length <= 20`
- `customerNamei` and `foodItemi` consist of lowercase and uppercase English letters and the space character.
- `tableNumberi` is a valid integer between `1` and `500`.

__题目描述:__

给你一个数组 `orders`，表示客户在餐厅中完成的订单，确切地说， `orders[i]=[customerNamei,tableNumberi,foodItemi]` ，其中 `customerNamei` 是客户的姓名，`tableNumberi` 是客户所在餐桌的桌号，而 `foodItemi` 是客户点的餐品名称。

请你返回该餐厅的 点菜展示表 。在这张表中，表中第一行为标题，其第一列为餐桌桌号 “Table” ，后面每一列都是按字母顺序排列的餐品名称。接下来每一行中的项则表示每张餐桌订购的相应餐品数量，第一列应当填对应的桌号，后面依次填写下单的餐品数量。

注意：客户姓名不是点菜展示表的一部分。此外，表中的数据行应该按餐桌桌号升序排列。

__示例:__

示例 1：

```text
输入：orders = [["David","3","Ceviche"],["Corina","10","Beef Burrito"],["David","3","Fried Chicken"],["Carla","5","Water"],["Carla","5","Ceviche"],["Rous","3","Ceviche"]]
输出：[["Table","Beef Burrito","Ceviche","Fried Chicken","Water"],["3","0","2","1","0"],["5","0","1","0","1"],["10","1","0","0","0"]] 
解释：
点菜展示表如下所示：
Table,Beef Burrito,Ceviche,Fried Chicken,Water
3    ,0           ,2      ,1            ,0
5    ,0           ,1      ,0            ,1
10   ,1           ,0      ,0            ,0
对于餐桌 3：David 点了 "Ceviche" 和 "Fried Chicken"，而 Rous 点了 "Ceviche"
而餐桌 5：Carla 点了 "Water" 和 "Ceviche"
餐桌 10：Corina 点了 "Beef Burrito"
```

示例 2：

```text
输入：orders = [["James","12","Fried Chicken"],["Ratesh","12","Fried Chicken"],["Amadeus","12","Fried Chicken"],["Adam","1","Canadian Waffles"],["Brianna","1","Canadian Waffles"]]
输出：[["Table","Canadian Waffles","Fried Chicken"],["1","2","0"],["12","0","3"]] 
解释：
对于餐桌 1：Adam 和 Brianna 都点了 "Canadian Waffles"
而餐桌 12：James, Ratesh 和 Amadeus 都点了 "Fried Chicken"
```

示例 3：

```text
输入：orders = [["Laura","2","Bean Burrito"],["Jhon","2","Beef Burrito"],["Melissa","2","Soda"]]
输出：[["Table","Bean Burrito","Beef Burrito","Soda"],["2","1","1","1"]]
```

__提示：__

- `1 <= orders.length <= 5 * 10 ^ 4`
- `orders[i].length == 3`
- `1 <= customerNamei.length, foodItemi.length <= 20`
- `customerNamei` 和 `foodItemi` 由大小写英文字母及空格字符 `' '` 组成。
- `tableNumberi` 是 `1` 到 `500` 范围内的整数。

__思路:__

```text
模拟
用一个哈希表记录每一个桌子及点的菜品数量
用另外两个哈希表记录桌子的序号以及菜品的名称
分别将两个哈希表排序
然后按照题目要求输出相应格式即可
时间复杂度为 O(L + NlogN + MlogM + MN), 空间复杂度为 O(L + M + N), 其中 L 表示 orders 数组的长度, N 表示餐品的数量, M 表示餐桌的数量
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<string>> displayTable(vector<vector<string>>& orders) 
    {
        vector<vector<string>> result;
        set<string> foods;       
        map<int, unordered_map<string, int>> m;
        for (auto order : orders)
        {
            ++m[stoi(order[1])][order[2]];  
            foods.insert(order[2]);            
        }
        vector<string> head(foods.begin(), foods.end());
        head.insert(head.begin(), "Table");
        result.push_back(head);
        for (auto& [k, v] : m)
        {
            vector<string> cur;
            cur.emplace_back(to_string(k));
            for (auto& food : foods) cur.emplace_back(to_string(v[food]));
            result.emplace_back(cur);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<String>> displayTable(List<List<String>> orders) {
        List<List<String>> result = new ArrayList<>();
        TreeSet<String> foods = new TreeSet<>();       
        TreeMap<Integer, Map<String, Integer>> map = new TreeMap<>();
        for (List<String> order : orders) {
            String food = order.get(2);
            Integer count = Integer.parseInt(order.get(1));
            if (!map.containsKey(count)) map.put(count, new HashMap<String, Integer>());
            Map<String, Integer> cur = map.get(count);
            cur.put(food, cur.getOrDefault(food, 0) + 1);
            map.put(count, cur);
            foods.add(food);
        }
        List<String> head = new ArrayList<>(foods);
        head.add(0, "Table");
        result.add(head);
        for (Map.Entry<Integer, Map<String, Integer>> e : map.entrySet()) {
            List<String> cur = new ArrayList<>();
            cur.add(String.valueOf(e.getKey()));
            for (String food : foods) cur.add(String.valueOf(e.getValue().getOrDefault(food, 0)));
            result.add(cur);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def displayTable(self, orders: List[List[str]]) -> List[List[str]]:
        d, tables, foods = defaultdict(lambda : defaultdict(int)), set(), set()
        for _, table, food in orders:
            d[table][food] += 1
            tables.add(table)
            foods.add(food)
        return [['Table'] + sorted(foods)] + [[table] + [str(d[table][food]) for food in sorted(foods)] for table in sorted(tables, key=int)]
```
