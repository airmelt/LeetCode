# 2286 Booking Concert Tickets in Groups 以组为单位订音乐会的门票

__Description:__

A concert hall has `n` rows numbered from `0` to `n - 1`, each with `m` seats, numbered from `0` to `m - 1`. You need to design a ticketing system that can allocate seats in the following cases:

- If a group of `k` spectators can sit __together__ in a row.
- If __every__ member of a group of `k` spectators can get a seat. They may or __may not__ sit together.

Note that the spectators are very picky. Hence:

- They will book seats only if each member of their group can get a seat with row number __less than or equal__ to `maxRow`. `maxRow` can __vary__ from group to group.
- In case there are multiple rows to choose from, the row with the __smallest__ number is chosen. If there are multiple seats to choose in the same row, the seat with the __smallest__ number is chosen.

Implement the `BookMyShow` class:

- `BookMyShow(int n, int m)` Initializes the object with `n` as number of rows and `m` as number of seats per row.
- `int[] gather(int k, int maxRow)` Returns an array of length `2` denoting the row and seat number (respectively) of the __first seat__ being allocated to the `k` members of the group, who must sit __together__. In other words, it returns the smallest possible `r` and `c` such that all `[c, c + k - 1]` seats are valid and empty in row `r`, and `r <= maxRow`. Returns `[]` in case it is __not possible__ to allocate seats to the group.
- `boolean scatter(int k, int maxRow)` Returns `true` if all `k` members of the group can be allocated seats in rows `0` to `maxRow`, who may or __may not__ sit together. If the seats can be allocated, it allocates `k` seats to the group with the __smallest__ row numbers, and the smallest possible seat numbers in each row. Otherwise, returns `false`.

__Example:__

Example 1:

```text
Input
["BookMyShow", "gather", "gather", "scatter", "scatter"]
[[2, 5], [4, 0], [2, 0], [5, 1], [5, 1]]
Output
[null, [0, 0], [], true, false]

Explanation
```

```Java
BookMyShow bms = new BookMyShow(2, 5); // There are 2 rows with 5 seats each 
bms.gather(4, 0); // return [0, 0]
                  // The group books seats [0, 3] of row 0. 
bms.gather(2, 0); // return []
                  // There is only 1 seat left in row 0,
                  // so it is not possible to book 2 consecutive seats. 
bms.scatter(5, 1); // return True
                   // The group books seat 4 of row 0 and seats [0, 3] of row 1. 
bms.scatter(5, 1); // return False
                   // There is only one seat left in the hall.
```

__Constraints:__

- `1 <= n <= 5 * 10 ^ 4`
- `1 <= m, k <= 10 ^ 9`
- `0 <= maxRow <= n - 1`
- At most `5 * 10 ^ 4` calls __in total__ will be made to `gather` and `scatter`.

__题目描述:__

一个音乐会总共有 `n` 排座位，编号从 `0` 到 `n - 1` ，每一排有 `m` 个座椅，编号为 `0` 到 `m - 1` 。你需要设计一个买票系统，针对以下情况进行座位安排:

- 同一组的 `k` 位观众坐在 __同一排座位，且座位连续__ 。
- `k` 位观众中 __每一位__ 都有座位坐，但他们 __不一定__ 坐在一起。

由于观众非常挑剔，所以：

- 只有当一个组里所有成员座位的排数都 __小于等于__ `maxRow` ，这个组才能订座位。每一组的 `maxRow` 可能 __不同__ 。
- 如果有多排座位可以选择，优先选择 __最小__ 的排数。如果同一排中有多个座位可以坐，优先选择号码 __最小__ 的。

请你实现 `BookMyShow` 类:

- `BookMyShow(int n, int m)` ，初始化对象， `n` 是排数， `m` 是每一排的座位数。
- `int[] gather(int k, int maxRow)` 返回长度为 `2` 的数组，表示 `k` 个成员中 __第一个座位__ 的排数和座位编号，这 `k` 位成员必须坐在 __同一排座位，且座位连续__ 。换言之，返回最小可能的 `r` 和 `c` 满足第 `r` 排中 `[c, c + k - 1]` 的座位都是空的，且 `r <= maxRow` 。如果 __无法__ 安排座位，返回 `[]` 。
- `boolean scatter(int k, int maxRow)` 如果组里所有 `k` 个成员 __不一定__ 要坐在一起的前提下，都能在第 `0` 排到第 `maxRow` 排之间找到座位，那么请返回 `true` 。这种情况下，每个成员都优先找排数 __最小__ ，然后是座位编号最小的座位。如果不能安排所有 `k` 个成员的座位，请返回 `false` 。

__示例:__

示例 1：

```text
输入：
["BookMyShow", "gather", "gather", "scatter", "scatter"]
[[2, 5], [4, 0], [2, 0], [5, 1], [5, 1]]
输出：
[null, [0, 0], [], true, false]

解释：
```

```Java
BookMyShow bms = new BookMyShow(2, 5); // 总共有 2 排，每排 5 个座位。
bms.gather(4, 0); // 返回 [0, 0]
                  // 这一组安排第 0 排 [0, 3] 的座位。
bms.gather(2, 0); // 返回 []
                  // 第 0 排只剩下 1 个座位。
                  // 所以无法安排 2 个连续座位。
bms.scatter(5, 1); // 返回 True
                   // 这一组安排第 0 排第 4 个座位和第 1 排 [0, 3] 的座位。
bms.scatter(5, 1); // 返回 False
                   // 总共只剩下 1 个座位。
```

__提示：__

- `1 <= n <= 5 * 10 ^ 4`
- `1 <= m, k <= 10 ^ 9`
- `0 <= maxRow <= n - 1`
- `gather` 和 `scatter` __总__ 调用次数不超过 `5 * 10 ^ 4` 次。

__思路:__

```text
线段树
用两个数组记录每一排被占用的座位数和总座位数
每次更新的时候单点更新
查询的时候查询区间和
对于连续函数 gather()
找第一个满足条件的排数的时候使用二分查找
为了找到有 k 个空位的最小排数, 需要找到占用的座位数小于等于 m - k 的最小排数
如果当前排数的占用座位数大于 m - k, 返回 -1
如果查找区间长度为 1, 返回当前排数
如果左半区间有满足 min_value[p << 1] <= m - k 的排数, 在左半区间递归查找
否则如果右半区间包括 maxRow, 在右半区间递归查找
否则返回 -1
找到之后分配座位
对于不连续函数 scatter()
首先判断是否有足够的座位
如果 sum_value[maxRow] > m * (maxRow + 1) - k, 返回 false
否则从第一个没满的排数开始分配座位
初始化时间复杂度为 O(N), gather() 函数时间复杂度为 O(logN), scatter() 函数时间复杂度为 O(logN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class BookMyShow 
{
public:
    BookMyShow(int n, int m) : n(n), m(m), min_value(n << 2), sum_value(n << 2) {}
    
    vector<int> gather(int k, int maxRow) 
    {
        int r = find_first(0, n - 1, 1, maxRow, m - k);
        if (r < 0) return {};
        long long c = query_sum(r, r, 0, n - 1, 1);
        update(0, n - 1, 1, r, k);
        return {r, (int)c};
    }
    
    bool scatter(int k, int maxRow) 
    {
        long long r = query_sum(0, maxRow, 0, n - 1, 1);
        if (r > (long long)m * (maxRow + 1) - k) return false;
        for (int i = find_first(0, n - 1, 1, maxRow, m - 1), left = 0; k > 0; i++, k -= left) 
        {
            left = min(m - (int)query_sum(i, i, 0, n - 1, 1), k);
            update(0, n - 1, 1, i, left);
        }
        return true;
    }
private:
    int m, n;
    vector<int> min_value;
    vector<long long> sum_value;

    void update(int s, int t, int p, int i, int val) 
    {
        if (s == t) 
        {
            min_value[p] += val;
            sum_value[p] += val;
            return;
        }
        int m = s + ((t - s) >> 1);
        if (i <= m) update(s, m, p << 1, i, val);
        else update(m + 1, t, (p << 1) + 1, i, val);
        min_value[p] = min(min_value[p << 1], min_value[(p << 1) + 1]);
        sum_value[p] = sum_value[p << 1] + sum_value[(p << 1) + 1];
    }

    long long query_sum(int l, int r, int s, int t, int p) 
    {
        if (l <= s and t <= r) return sum_value[p];
        long long result = 0, m = s + ((t - s) >> 1);
        if (l <= m) result += query_sum(l, r, s, m, p << 1);
        if (r > m) result += query_sum(l, r, m + 1, t, (p << 1) + 1);
        return result;
    }

    int find_first(int s, int t, int p, int maxRow, int val) 
    {
        if (min_value[p] > val) return -1;
        if (s == t) return s;
        int m = s + ((t - s) >> 1);
        if (min_value[p << 1] <= val) return find_first(s, m, p << 1, maxRow, val);
        if (maxRow > m) return find_first(m + 1, t, (p << 1) + 1, maxRow, val);
        return -1;
    }
};

/**
 * Your BookMyShow object will be instantiated and called as such:
 * BookMyShow* obj = new BookMyShow(n, m);
 * vector<int> param_1 = obj->gather(k,maxRow);
 * bool param_2 = obj->scatter(k,maxRow);
 */
```

__Java__:

```Java
class BookMyShow {
    private int m, n;
    private int[] minValue;
    private long[] sumValue;

    public BookMyShow(int n, int m) {
        this.m = m;
        this.n = n;
        this.minValue = new int[n << 2];
        this.sumValue = new long[n << 2];
    }

    private void update(int s, int t, int p, int i, int val) {
        if (s == t) {
            minValue[p] += val;
            sumValue[p] += val;
            return;
        }
        int m = s + ((t - s) >>> 1);
        if (i <= m) update(s, m, p << 1, i, val);
        else update(m + 1, t, (p << 1) + 1, i, val);
        minValue[p] = Math.min(minValue[p << 1], minValue[(p << 1) + 1]);
        sumValue[p] = sumValue[p << 1] + sumValue[(p << 1) + 1];
    }

    private long querySum(int l, int r, int s, int t, int p) {
        if (l <= s && t <= r) return sumValue[p];
        long result = 0;
        int m = s + ((t - s) >>> 1);
        if (l <= m) result += querySum(l, r, s, m, p << 1);
        if (r > m) result += querySum(l, r, m + 1, t, (p << 1) + 1);
        return result;
    }

    private int findFirst(int s, int t, int p, int maxRow, int val) {
        if (minValue[p] > val) return -1;
        if (s == t) return s;
        int m = s + ((t - s) >>> 1);
        if (minValue[p << 1] <= val) return findFirst(s, m, p << 1, maxRow, val);
        if (maxRow > m) return findFirst(m + 1, t, (p << 1) + 1, maxRow, val);
        return -1;
    }
    
    public int[] gather(int k, int maxRow) {
        int r = findFirst(0, n - 1, 1, maxRow, m - k);
        if (r < 0) return new int[0];
        long c = querySum(r, r, 0, n - 1, 1);
        update(0, n - 1, 1, r, k);
        return new int[]{r, (int)c};
    }
    
    public boolean scatter(int k, int maxRow) {
        long r = querySum(0, maxRow, 0, n - 1, 1);
        if (r > (long)m * (maxRow + 1) - k) return false;
        for (int i = findFirst(0, n - 1, 1, maxRow, m - 1), left = 0; k > 0; i++, k -= left) {
            left = Math.min(m - (int)querySum(i, i, 0, n - 1, 1), k);
            update(0, n - 1, 1, i, left);
        }
        return true;
    }
}

/**
 * Your BookMyShow object will be instantiated and called as such:
 * BookMyShow obj = new BookMyShow(n, m);
 * int[] param_1 = obj.gather(k,maxRow);
 * boolean param_2 = obj.scatter(k,maxRow);
 */
```

__Python__:

```Python
class BookMyShow:
    def __init__(self, n: int, m: int):
        self.m = m
        self.n = n
        self.min_value = [0] * (2 << n.bit_length())
        self.sum_value = [0] * (2 << n.bit_length())


    def update(self, s: int, t: int, p: int, i: int, val: int) -> None:
        if s == t:
            self.min_value[p] += val
            self.sum_value[p] += val
            return
        m = s + ((t - s) >> 1)
        if i <= m:
            self.update(s, m, p << 1, i, val)
        else:
            self.update(m + 1, t, (p << 1) + 1, i, val)
        self.min_value[p] = min(self.min_value[p << 1], self.min_value[(p << 1) + 1])
        self.sum_value[p] = self.sum_value[p << 1] + self.sum_value[(p << 1) + 1]
        

    def query_sum(self, l: int, r: int, s: int, t: int, p: int) -> int:
        if l <= s and t <= r:
            return self.sum_value[p]
        result, m = 0, s + ((t - s) >> 1)
        if l <= m:
            result += self.query_sum(l, r, s, m, p << 1)
        if r > m:
            result += self.query_sum(l, r, m + 1, t, (p << 1) + 1)
        return result


    def find_first(self, s: int, t: int, p: int, maxRow: int, val: int) -> int:
        if self.min_value[p] > val:
            return -1
        if s == t:
            return s
        m = s + ((t - s) >> 1)
        if self.min_value[p << 1] <= val:
            return self.find_first(s, m, p << 1, maxRow, val)
        if maxRow > m:
            return self.find_first(m + 1, t, (p << 1) + 1, maxRow, val)
        return -1


    def gather(self, k: int, maxRow: int) -> List[int]:
        if (r := self.find_first(0, self.n - 1, 1, maxRow, self.m - k)) < 0:
            return []
        c = self.query_sum(r, r, 0, self.n - 1, 1)
        self.update(0, self.n - 1, 1, r, k)
        return [r, c]


    def scatter(self, k: int, maxRow: int) -> bool:
        if (r := self.query_sum(0, maxRow, 0, self.n - 1, 1)) > self.m * (maxRow + 1) - k:
            return False
        i = self.find_first(0, self.n - 1, 1, maxRow, self.m - 1)
        while k:
            left = min(self.m - self.query_sum(i, i, 0, self.n - 1, 1), k)
            self.update(0, self.n - 1, 1, i, left)
            k -= left
            i += 1
        return True



# Your BookMyShow object will be instantiated and called as such:
# obj = BookMyShow(n, m)
# param_1 = obj.gather(k,maxRow)
# param_2 = obj.scatter(k,maxRow)
```
