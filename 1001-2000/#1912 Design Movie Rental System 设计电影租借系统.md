# 1912 Design Movie Rental System 设计电影租借系统

__Description:__

You have a movie renting company consisting of `n` shops. You want to implement a renting system that supports searching for, booking, and returning movies. The system should also support generating a report of the currently rented movies.

Each movie is given as a 2D integer array `entries` where `entries[i] = [shopi, moviei, pricei]` indicates that there is a copy of movie `moviei` at shop `shopi` with a rental price of `pricei`. Each shop carries __at most one__ copy of a movie `moviei`.

The system should support the following functions:

- __Search__: Finds the __cheapest 5 shops__ that have an __unrented copy__ of a given movie. The shops should be sorted by __price__ in ascending order, and in case of a tie, the one with the __smaller__ `shopi` should appear first. If there are less than 5 matching shops, then all of them should be returned. If no shop has an unrented copy, then an empty list should be returned.
- __Rent__: Rents an __unrented copy__ of a given movie from a given shop.
- __Drop__: Drops off a __previously rented copy__ of a given movie at a given shop.
- __Report__: Returns the __cheapest 5 rented movies__ (possibly of the same movie ID) as a 2D list `res` where `res[j] = [shopj, moviej]` describes that the `j ^ th` cheapest rented movie `moviej` was rented from the shop `shopj`. The movies in `res` should be sorted by __price__ in ascending order, and in case of a tie, the one with the __smaller__ `shopj` should appear first, and if there is still tie, the one with the __smaller__ `moviej` should appear first. If there are fewer than 5 rented movies, then all of them should be returned. If no movies are currently being rented, then an empty list should be returned.

Implement the `MovieRentingSystem` class:

- `MovieRentingSystem(int n, int[][] entries)` Initializes the `MovieRentingSystem` object with `n` shops and the movies in `entries`.
- `List<Integer> search(int movie)` Returns a list of shops that have an __unrented copy__ of the given `movie` as described above.
- `void rent(int shop, int movie)` Rents the given `movie` from the given `shop`.
- `void drop(int shop, int movie)` Drops off a previously rented `movie` at the given `shop`.
- `List<List<Integer>> report()` Returns a list of cheapest __rented__ movies as described above.

__Note:__ The test cases will be generated such that `rent` will only be called if the shop has an __unrented__ copy of the movie, and `drop` will only be called if the shop had __previously rented__ out the movie.

__Example:__

Example 1:

```text
Input
["MovieRentingSystem", "search", "rent", "rent", "report", "drop", "search"]
[[3, [[0, 1, 5], [0, 2, 6], [0, 3, 7], [1, 1, 4], [1, 2, 7], [2, 1, 5]]], [1], [0, 1], [1, 2], [], [1, 2], [2]]
Output
[null, [1, 0, 2], null, null, [[0, 1], [1, 2]], null, [0, 1]]

Explanation
```

```Java
MovieRentingSystem movieRentingSystem = new MovieRentingSystem(3, [[0, 1, 5], [0, 2, 6], [0, 3, 7], [1, 1, 4], [1, 2, 7], [2, 1, 5]]);
movieRentingSystem.search(1);  // return [1, 0, 2], Movies of ID 1 are unrented at shops 1, 0, and 2. Shop 1 is cheapest; shop 0 and 2 are the same price, so order by shop number.
movieRentingSystem.rent(0, 1); // Rent movie 1 from shop 0. Unrented movies at shop 0 are now [2,3].
movieRentingSystem.rent(1, 2); // Rent movie 2 from shop 1. Unrented movies at shop 1 are now [1].
movieRentingSystem.report();   // return [[0, 1], [1, 2]]. Movie 1 from shop 0 is cheapest, followed by movie 2 from shop 1.
movieRentingSystem.drop(1, 2); // Drop off movie 2 at shop 1. Unrented movies at shop 1 are now [1,2].
movieRentingSystem.search(2);  // return [0, 1]. Movies of ID 2 are unrented at shops 0 and 1. Shop 0 is cheapest, followed by shop 1.
```

__Constraints:__

- `1 <= n <= 3 * 10 ^ 5`
- `1 <= entries.length <= 10 ^ 5`
- `0 <= shopi < n`
- `1 <= moviei, pricei <= 10 ^ 4`
- Each shop carries __at most one__ copy of a movie `moviei`.
- At most `10 ^ 5` calls __in total__ will be made to `search`, `rent`, `drop` and `report`.

__题目描述:__

你有一个电影租借公司和 `n` 个电影商店。你想要实现一个电影租借系统，它支持查询、预订和返还电影的操作。同时系统还能生成一份当前被借出电影的报告。

所有电影用二维整数数组 `entries` 表示，其中 `entries[i] = [shopi, moviei, pricei]` 表示商店 `shopi` 有一份电影 `moviei` 的拷贝，租借价格为 `pricei` 。每个商店有 __至多一份__ 编号为 `moviei` 的电影拷贝。

系统需要支持以下操作：

__Search:__ 找到拥有指定电影且 __未借出__ 的商店中 __最便宜的 5 个__ 。商店需要按照 __价格__ 升序排序，如果价格相同，则 `shopi` __较小__ 的商店排在前面。如果查询结果少于 5 个商店，则将它们全部返回。如果查询结果没有任何商店，则返回空列表。
__Rent:__ 从指定商店借出指定电影，题目保证指定电影在指定商店 __未借出__ 。
__Drop:__ 在指定商店返还 __之前已借出__ 的指定电影。
__Report:__ 返回 __最便宜的 5 部已借出电影__ （可能有重复的电影 ID），将结果用二维列表 `res` 返回，其中 `res[j] = [shopj, moviej]` 表示第 `j` 便宜的已借出电影是从商店 `shopj` 借出的电影 `moviej` 。 `res` 中的电影需要按 __价格__ 升序排序；如果价格相同，则 `shopj` __较小__ 的排在前面；如果仍然相同，则 `moviej` __较小__ 的排在前面。如果当前借出的电影小于 5 部，则将它们全部返回。如果当前没有借出电影，则返回一个空的列表。

请你实现 `MovieRentingSystem` 类:

- `MovieRentingSystem(int n, int[][] entries)` 将 `MovieRentingSystem` 对象用 `n` 个商店和 `entries` 表示的电影列表初始化。
- `List<Integer> search(int movie)` 如上所述，返回 __未借出__ 指定 `movie` 的商店列表。
- `void rent(int shop, int movie)` 从指定商店 `shop` 借出指定电影 `movie` 。
- `void drop(int shop, int movie)` 在指定商店 `shop` 返还之前借出的电影 `movie` 。
- `List<List<Integer>> report()` 如上所述，返回最便宜的 __已借出__ 电影列表。

__注意:__ 测试数据保证 `rent` 操作中指定商店拥有 __未借出__ 的指定电影，且 `drop` 操作指定的商店 __之前已借出__ 指定电影。

__示例:__

示例 1：

```text
输入：
["MovieRentingSystem", "search", "rent", "rent", "report", "drop", "search"]
[[3, [[0, 1, 5], [0, 2, 6], [0, 3, 7], [1, 1, 4], [1, 2, 7], [2, 1, 5]]], [1], [0, 1], [1, 2], [], [1, 2], [2]]
输出：
[null, [1, 0, 2], null, null, [[0, 1], [1, 2]], null, [0, 1]]

解释：
```

```Java
MovieRentingSystem movieRentingSystem = new MovieRentingSystem(3, [[0, 1, 5], [0, 2, 6], [0, 3, 7], [1, 1, 4], [1, 2, 7], [2, 1, 5]]);
movieRentingSystem.search(1);  // 返回 [1, 0, 2] ，商店 1，0 和 2 有未借出的 ID 为 1 的电影。商店 1 最便宜，商店 0 和 2 价格相同，所以按商店编号排序。
movieRentingSystem.rent(0, 1); // 从商店 0 借出电影 1 。现在商店 0 未借出电影编号为 [2,3] 。
movieRentingSystem.rent(1, 2); // 从商店 1 借出电影 2 。现在商店 1 未借出的电影编号为 [1] 。
movieRentingSystem.report();   // 返回 [[0, 1], [1, 2]] 。商店 0 借出的电影 1 最便宜，然后是商店 1 借出的电影 2 。
movieRentingSystem.drop(1, 2); // 在商店 1 返还电影 2 。现在商店 1 未借出的电影编号为 [1,2] 。
movieRentingSystem.search(2);  // 返回 [0, 1] 。商店 0 和 1 有未借出的 ID 为 2 的电影。商店 0 最便宜，然后是商店 1 。
```

__提示：__

- `1 <= n <= 3 * 10 ^ 5`
- `1 <= entries.length <= 10 ^ 5`
- `0 <= shopi < n`
- `1 <= moviei, pricei <= 10 ^ 4`
- 每个商店 __至多__ 有一份电影 `moviei` 的拷贝。
- `search`， `rent`， `drop` 和 `report` 的调用 __总共__ 不超过 `10 ^ 5` 次。

__思路:__

```text
有序数组
用哈希表 movies 记录每部电影的价格, 编号和商店
为了用一个数字记录下电影的商店和编号, 我们可以用一个 32 位的数字, 前 16 位记录商店, 后 16 位记录电影
用哈希表 unrented 记录未借出的电影编号对应的的价格和商店, 我们可以用一个 64 位的数字, 前 32 位记录价格, 后 32 位记录商店
用一个有序数组 rented 记录已借出的电影, 我们可以用一个 64 位的数字, 前 32 位记录价格, 中间 16 位记录商店, 后 16 位记录电影
search: 从哈希表 unrented 中取出前 5 个数字, 直到取出 5 个数字或者哈希表 unrented 为空
rent: 从哈希表 unrented 中删除对应的数字, 并将其加入到有序数组 rented 中
drop: 从有序数组 rented 中删除对应的数字, 并将其加入到哈希表 unrented 中
report: 从有序数组 rented 中取出前 5 个数字, 直到取出 5 个数字或者有序数组 rented 为空
MovieRentingSystem 构造函数时间复杂度为 O(N)
search, rent, drop, report 的时间复杂度为 O(logN)
空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class MovieRentingSystem 
{
public:
    MovieRentingSystem(int n, vector<vector<int>>& entries) 
    {
        for (auto& entry : entries) 
        {
            long long shop = entry[0], movie = entry[1], price = entry[2];
            movies[(shop << 16) | movie] = price;
            unrented[movie].insert((price << 32) | shop);
        }
    }

    vector<int> search(int movie) 
    {
        vector<int> result;
        for (auto iter = unrented[movie].begin(); iter != unrented[movie].end() && result.size() != 5; ++iter) result.push_back((int)(*iter));
        return result;
    }

    void rent(int shop, int movie) 
    {
        long long price = movies[((long long)shop << 16) | movie];
        unrented[movie].erase((price << 32) | shop);
        rented.insert((price << 48) | ((long long)shop << 16) | movie);
    }

    void drop(int shop, int movie) 
    {
        long long price = movies[((long long)shop << 16) | movie];
        rented.erase((price << 48) | ((long long)shop << 16) | movie);
        unrented[movie].insert((price << 32) | shop);
    }

    vector<vector<int>> report() 
    {
        vector<vector<int>> result;
        for (auto iter = rented.begin(); iter != rented.end() && result.size() != 5; ++iter) result.push_back({ (int)(*iter >> 16), (int)(*iter) & 0xffff });
        return result;
    }

private:
    unordered_map<long long, long long> movies;
    unordered_map<long long, set<long long>> unrented;
    set<long long> rented;
};

/**
 * Your MovieRentingSystem object will be instantiated and called as such:
 * MovieRentingSystem* obj = new MovieRentingSystem(n, entries);
 * vector<int> param_1 = obj->search(movie);
 * obj->rent(shop,movie);
 * obj->drop(shop,movie);
 * vector<vector<int>> param_4 = obj->report();
 */
```

__Java__:

```Java
class MovieRentingSystem {
    private Map<Long, Long> movies = new HashMap<>();
    private Map<Long, Set<Long>> unrented = new HashMap<>();
    private Set<Long> rented = new TreeSet<>();

    public MovieRentingSystem(int n, int[][] entries) {
        for (int[] entry : entries) {
            long shop = entry[0], movie = entry[1], price = entry[2];
            movies.put((shop << 16) | movie, price);
            unrented.putIfAbsent(movie, new TreeSet<>());
            unrented.get(movie).add((price << 32) | shop);
        }
    }
    
    public List<Integer> search(int movie) {
        List<Integer> result = new ArrayList<>();
        for (long cur : unrented.getOrDefault((long)movie, new TreeSet<>())) {
            result.add((int)cur);
            if (result.size() == 5) break;
        }
        return result;
    }
    
    public void rent(int shop, int movie) {
        long price = movies.get(((long)shop << 16) | movie);
        unrented.getOrDefault((long)movie, new TreeSet<>()).remove((price << 32) | shop);
        rented.add((price << 48) | ((long)shop << 16) | movie);
    }
    
    public void drop(int shop, int movie) {
        long price = movies.get(((long)shop << 16) | movie);
        rented.remove((price << 48) | ((long)shop << 16) | movie);
        unrented.putIfAbsent((long)movie, new TreeSet<>());
        unrented.get((long)movie).add((price << 32) | shop);
    }
    
    public List<List<Integer>> report() {
        List<List<Integer>> result = new ArrayList<>();
        for (long cur : rented) {
            List<Integer> movie = new ArrayList<>(){{
                add((int)(cur >> 16));
                add((int)(cur & 0xffff));
            }};
            result.add(movie);
            if (result.size() == 5) break;
        }
        return result;
    }
}

/**
 * Your MovieRentingSystem object will be instantiated and called as such:
 * MovieRentingSystem obj = new MovieRentingSystem(n, entries);
 * List<Integer> param_1 = obj.search(movie);
 * obj.rent(shop,movie);
 * obj.drop(shop,movie);
 * List<List<Integer>> param_4 = obj.report();
 */
```

__Python__:

```Python
from sortedcontainers import SortedList
class MovieRentingSystem:

    def __init__(self, n: int, entries: List[List[int]]):
        self.movies = defaultdict(int)
        self.unrented = defaultdict(SortedList)
        self.rented = SortedList()
        for shop, movie, price in entries:
            self.movies[(shop, movie)] = price
            self.unrented[movie].add((price, shop))


    def search(self, movie: int) -> List[int]:
        return [shop for price, shop in self.unrented[movie][:5]]


    def rent(self, shop: int, movie: int) -> None:
        self.unrented[movie].discard((price := self.movies[(shop, movie)], shop))
        self.rented.add((price, shop, movie))


    def drop(self, shop: int, movie: int) -> None:
        self.unrented[movie].add((price := self.movies[(shop, movie)], shop))
        self.rented.discard((price, shop, movie))


    def report(self) -> List[List[int]]:
        return [(shop, movie) for price, shop, movie in self.rented[:5]]



# Your MovieRentingSystem object will be instantiated and called as such:
# obj = MovieRentingSystem(n, entries)
# param_1 = obj.search(movie)
# obj.rent(shop,movie)
# obj.drop(shop,movie)
# param_4 = obj.report()
```
