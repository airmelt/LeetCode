# 2353 Design a Food Rating System 设计食物评分系统

__Description:__

Design a food rating system that can do the following:

- __Modify__ the rating of a food item listed in the system.
- Return the highest-rated food item for a type of cuisine in the system.

Implement the `FoodRatings` class:

- `FoodRatings(String[] foods, String[] cuisines, int[] ratings)` Initializes the system. The food items are described by `foods`, `cuisines` and `ratings`, all of which have a length of `n`.
- `foods[i]` is the name of the `i ^ th` food,
- `cuisines[i]` is the type of cuisine of the `i ^ th` food, and
- `ratings[i]` is the initial rating of the `i ^ th` food.
- `void changeRating(String food, int newRating)` Changes the rating of the food item with the name `food`.
- `String highestRated(String cuisine)` Returns the name of the food item that has the highest rating for the given type of `cuisine`. If there is a tie, return the item with the __lexicographically smaller__ name.

Note that a string `x` is lexicographically smaller than string `y` if `x` comes before `y` in dictionary order, that is, either `x` is a prefix of `y`, or if `i` is the first position such that `x[i] != y[i]`, then `x[i]` comes before `y[i]` in alphabetic order.

__Example:__

Example 1:

```text
Input
["FoodRatings", "highestRated", "highestRated", "changeRating", "highestRated", "changeRating", "highestRated"]
[[["kimchi", "miso", "sushi", "moussaka", "ramen", "bulgogi"], ["korean", "japanese", "japanese", "greek", "japanese", "korean"], [9, 12, 8, 15, 14, 7]], ["korean"], ["japanese"], ["sushi", 16], ["japanese"], ["ramen", 16], ["japanese"]]
Output
[null, "kimchi", "ramen", null, "sushi", null, "ramen"]

Explanation
```

```Java
FoodRatings foodRatings = new FoodRatings(["kimchi", "miso", "sushi", "moussaka", "ramen", "bulgogi"], ["korean", "japanese", "japanese", "greek", "japanese", "korean"], [9, 12, 8, 15, 14, 7]);
foodRatings.highestRated("korean"); // return "kimchi"
                                    // "kimchi" is the highest rated korean food with a rating of 9.
foodRatings.highestRated("japanese"); // return "ramen"
                                      // "ramen" is the highest rated japanese food with a rating of 14.
foodRatings.changeRating("sushi", 16); // "sushi" now has a rating of 16.
foodRatings.highestRated("japanese"); // return "sushi"
                                      // "sushi" is the highest rated japanese food with a rating of 16.
foodRatings.changeRating("ramen", 16); // "ramen" now has a rating of 16.
foodRatings.highestRated("japanese"); // return "ramen"
                                      // Both "sushi" and "ramen" have a rating of 16.
                                      // However, "ramen" is lexicographically smaller than "sushi".
```

__Constraints:__

- `1 <= n <= 2 * 10 ^ 4`
- `n == foods.length == cuisines.length == ratings.length`
- `1 <= foods[i].length, cuisines[i].length <= 10`
- `foods[i]`, `cuisines[i]` consist of lowercase English letters.
- `1 <= ratings[i] <= 10 ^ 8`
- All the strings in `foods` are __distinct__.
- `food` will be the name of a food item in the system across all calls to `changeRating`.
- `cuisine` will be a type of cuisine of __at least one__ food item in the system across all calls to `highestRated`.
- At most `2 * 10 ^ 4` calls __in total__ will be made to `changeRating` and `highestRated`.

__题目描述:__

设计一个支持下述操作的食物评分系统：

- __修改__ 系统中列出的某种食物的评分。
- 返回系统中某一类烹饪方式下评分最高的食物。

实现 `FoodRatings` 类:

- `FoodRatings(String[] foods, String[] cuisines, int[] ratings)` 初始化系统。食物由 `foods`、 `cuisines` 和 `ratings` 描述，长度均为 `n` 。
- `foods[i]` 是第 `i` 种食物的名字。
- `cuisines[i]` 是第 `i` 种食物的烹饪方式。
- `ratings[i]` 是第 `i` 种食物的最初评分。
- `void changeRating(String food, int newRating)` 修改名字为 `food` 的食物的评分。
- `String highestRated(String cuisine)` 返回指定烹饪方式 `cuisine` 下评分最高的食物的名字。如果存在并列，返回 __字典序较小__ 的名字。

注意，字符串 `x` 的字典序比字符串 `y` 更小的前提是: `x` 在字典中出现的位置在 `y` 之前，也就是说，要么 `x` 是 `y` 的前缀，或者在满足 `x[i] != y[i]` 的第一个位置 `i` 处， `x[i]` 在字母表中出现的位置在 `y[i]` 之前。

__示例:__

示例：

```text
输入
["FoodRatings", "highestRated", "highestRated", "changeRating", "highestRated", "changeRating", "highestRated"]
[[["kimchi", "miso", "sushi", "moussaka", "ramen", "bulgogi"], ["korean", "japanese", "japanese", "greek", "japanese", "korean"], [9, 12, 8, 15, 14, 7]], ["korean"], ["japanese"], ["sushi", 16], ["japanese"], ["ramen", 16], ["japanese"]]
输出
[null, "kimchi", "ramen", null, "sushi", null, "ramen"]

解释
```

```Java
FoodRatings foodRatings = new FoodRatings(["kimchi", "miso", "sushi", "moussaka", "ramen", "bulgogi"], ["korean", "japanese", "japanese", "greek", "japanese", "korean"], [9, 12, 8, 15, 14, 7]);
foodRatings.highestRated("korean"); // 返回 "kimchi"
                                    // "kimchi" 是分数最高的韩式料理，评分为 9 。
foodRatings.highestRated("japanese"); // 返回 "ramen"
                                      // "ramen" 是分数最高的日式料理，评分为 14 。
foodRatings.changeRating("sushi", 16); // "sushi" 现在评分变更为 16 。
foodRatings.highestRated("japanese"); // 返回 "sushi"
                                      // "sushi" 是分数最高的日式料理，评分为 16 。
foodRatings.changeRating("ramen", 16); // "ramen" 现在评分变更为 16 。
foodRatings.highestRated("japanese"); // 返回 "ramen"
                                      // "sushi" 和 "ramen" 的评分都是 16 。
                                      // 但是，"ramen" 的字典序比 "sushi" 更小。
```

__提示：__

- `1 <= n <= 2 * 10 ^ 4`
- `n == foods.length == cuisines.length == ratings.length`
- `1 <= foods[i].length, cuisines[i].length <= 10`
- `foods[i]`、 `cuisines[i]` 由小写英文字母组成
- `1 <= ratings[i] <= 10 ^ 8`
- `foods` 中的所有字符串 __互不相同__
- 在对 `changeRating` 的所有调用中， `food` 是系统中食物的名字。
- 在对 `highestRated` 的所有调用中， `cuisine` 是系统中 __至少一种__ 食物的烹饪方式。
- 最多调用 `changeRating` 和 `highestRated` __总计__ `2 * 10 ^ 4` 次

__思路:__

```text
哈希表 ➕ 优先队列
用哈希表存储每一种食物的评分和烹饪方式
再用另一个哈希表存储每一种烹饪方式下的食物
其中评分和食物使用优先队列存储, 优先队列按照评分从大到小排序, 所以记录时需要取负值, 食物按照字典序排序
修改评分时直接将新的评分和食物加入到对应的优先队列中, 并更新食物哈希表
返回最高得分时先检查对应的食物分数是否发生改变, 如果改变则弹出, 直到相等
changeRating() 和 highestRated() 时间复杂度均为 O(logN), 空间复杂度为 O(N), N 为食物的数量, 即初始化时的长度
```

__代码:__

__C++__:

```C++
class FoodRatings 
{
public:
    FoodRatings(vector<string>& foods, vector<string>& cuisines, vector<int>& ratings) 
    {
        for (int i = 0, n = foods.size(); i < n; i++)
        {
            this -> foods[foods[i]] = {ratings[i], cuisines[i]};
            this -> cuisines[cuisines[i]].emplace(-ratings[i], foods[i]);
        }
    }
    
    void changeRating(string food, int newRating) 
    {
        auto& [r, c] = foods[food];
        cuisines[c].emplace(-newRating, food);
        r = newRating;
    }
    
    string highestRated(string cuisine) 
    {
        auto& h = cuisines[cuisine];
        while (-h.top().first != foods[h.top().second].first) h.pop();
        return h.top().second;
    }
private:
    unordered_map<string, pair<int, string>> foods;
    unordered_map<string, priority_queue<pair<int, string>, vector<pair<int, string>>, greater<>>> cuisines;
};

/**
 * Your FoodRatings object will be instantiated and called as such:
 * FoodRatings* obj = new FoodRatings(foods, cuisines, ratings);
 * obj->changeRating(food,newRating);
 * string param_2 = obj->highestRated(cuisine);
 */
```

__Java__:

```Java
class FoodRatings {
    Map<String, Pair<Integer, String>> foods = new HashMap<>();
    Map<String, Queue<Pair<Integer, String>>> cuisines = new HashMap<>();

    public FoodRatings(String[] foods, String[] cuisines, int[] ratings) {
        for (int i = 0, n = foods.length; i < n; i++) {
            this.foods.put(foods[i], new Pair<>(ratings[i], cuisines[i]));
            this.cuisines.computeIfAbsent(cuisines[i], k -> new PriorityQueue<>((a, b) -> !Objects.equals(a.getKey(), b.getKey()) ? b.getKey() - a.getKey() : a.getValue().compareTo(b.getValue()))).add(new Pair<>(ratings[i], foods[i]));
        }
    }
    
    public void changeRating(String food, int newRating) {
        var c = foods.get(food).getValue();
        cuisines.get(c).offer(new Pair<>(newRating, food));
        foods.put(food, new Pair<>(newRating, c));
    }
    
    public String highestRated(String cuisine) {
        var h = cuisines.get(cuisine);
        while (!Objects.equals(h.peek().getKey(), foods.get(h.peek().getValue()).getKey())) h.poll();
        return h.peek().getValue();
    }
}

/**
 * Your FoodRatings object will be instantiated and called as such:
 * FoodRatings obj = new FoodRatings(foods, cuisines, ratings);
 * obj.changeRating(food,newRating);
 * String param_2 = obj.highestRated(cuisine);
 */
```

__Python__:

```Python
class FoodRatings:
    def __init__(self, foods: List[str], cuisines: List[str], ratings: List[int]):
        self.foods = {}
        self.cuisines = defaultdict(list)
        for f, c, r in zip(foods, cuisines, ratings):
            self.foods[f] = [r, c]
            heappush(self.cuisines[c], (-r, f))


    def changeRating(self, food: str, newRating: int) -> None:
        f = self.foods[food]
        heappush(self.cuisines[f[1]], (-newRating, food))
        f[0] = newRating


    def highestRated(self, cuisine: str) -> str:
        h = self.cuisines[cuisine]
        while -h[0][0] != self.foods[h[0][1]][0]:
            heappop(h)
        return h[0][1]



# Your FoodRatings object will be instantiated and called as such:
# obj = FoodRatings(foods, cuisines, ratings)
# obj.changeRating(food,newRating)
# param_2 = obj.highestRated(cuisine)
```
