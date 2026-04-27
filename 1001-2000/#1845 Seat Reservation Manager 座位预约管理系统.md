# 1845 Seat Reservation Manager 座位预约管理系统

__Description:__

Design a system that manages the reservation state of `n` seats that are numbered from `1` to `n`.

Implement the `SeatManager` class:

- `SeatManager(int n)` Initializes a `SeatManager` object that will manage `n` seats numbered from `1` to `n`. All seats are initially available.
- `int reserve()` Fetches the __smallest-numbered__ unreserved seat, reserves it, and returns its number.
- `void unreserve(int seatNumber)` Unreserves the seat with the given `seatNumber`.

__Example:__

Example 1:

```text
Input
["SeatManager", "reserve", "reserve", "unreserve", "reserve", "reserve", "reserve", "reserve", "unreserve"]
[[5], [], [], [2], [], [], [], [], [5]]
Output
[null, 1, 2, null, 2, 3, 4, 5, null]

Explanation
SeatManager seatManager = new SeatManager(5); // Initializes a SeatManager with 5 seats.
seatManager.reserve();    // All seats are available, so return the lowest numbered seat, which is 1.
seatManager.reserve();    // The available seats are [2,3,4,5], so return the lowest of them, which is 2.
seatManager.unreserve(2); // Unreserve seat 2, so now the available seats are [2,3,4,5].
seatManager.reserve();    // The available seats are [2,3,4,5], so return the lowest of them, which is 2.
seatManager.reserve();    // The available seats are [3,4,5], so return the lowest of them, which is 3.
seatManager.reserve();    // The available seats are [4,5], so return the lowest of them, which is 4.
seatManager.reserve();    // The only available seat is seat 5, so return 5.
seatManager.unreserve(5); // Unreserve seat 5, so now the available seats are [5].
```

__Constraints:__

- `1 <= n <= 10 ^ 5`
- `1 <= seatNumber <= n`
- For each call to `reserve`, it is guaranteed that there will be at least one unreserved seat.
- For each call to `unreserve`, it is guaranteed that `seatNumber` will be reserved.
- At most `10 ^ 5` calls __in total__ will be made to `reserve` and `unreserve`.

__题目描述:__

请你设计一个管理 `n` 个座位预约的系统，座位编号从 `1` 到 `n` 。

请你实现 `SeatManager` 类:

- `SeatManager(int n)` 初始化一个 `SeatManager` 对象，它管理从 `1` 到 `n` 编号的 `n` 个座位。所有座位初始都是可预约的。
- `int reserve()` 返回可以预约座位的 __最小编号__ ，此座位变为不可预约。
- `void unreserve(int seatNumber)` 将给定编号 `seatNumber` 对应的座位变成可以预约。

__示例:__

示例 1：

```text
输入：
["SeatManager", "reserve", "reserve", "unreserve", "reserve", "reserve", "reserve", "reserve", "unreserve"]
[[5], [], [], [2], [], [], [], [], [5]]
输出：
[null, 1, 2, null, 2, 3, 4, 5, null]

解释：
SeatManager seatManager = new SeatManager(5); // 初始化 SeatManager ，有 5 个座位。
seatManager.reserve();    // 所有座位都可以预约，所以返回最小编号的座位，也就是 1 。
seatManager.reserve();    // 可以预约的座位为 [2,3,4,5] ，返回最小编号的座位，也就是 2 。
seatManager.unreserve(2); // 将座位 2 变为可以预约，现在可预约的座位为 [2,3,4,5] 。
seatManager.reserve();    // 可以预约的座位为 [2,3,4,5] ，返回最小编号的座位，也就是 2 。
seatManager.reserve();    // 可以预约的座位为 [3,4,5] ，返回最小编号的座位，也就是 3 。
seatManager.reserve();    // 可以预约的座位为 [4,5] ，返回最小编号的座位，也就是 4 。
seatManager.reserve();    // 唯一可以预约的是座位 5 ，所以返回 5 。
seatManager.unreserve(5); // 将座位 5 变为可以预约，现在可预约的座位为 [5] 。
```

__提示：__

- `1 <= n <= 10 ^ 5`
- `1 <= seatNumber <= n`
- 每一次对 `reserve` 的调用，题目保证至少存在一个可以预约的座位。
- 每一次对 `unreserve` 的调用，题目保证 `seatNumber` 在调用函数前都是被预约状态。
- 对 `reserve` 和 `unreserve` 的调用 __总共__ 不超过 `10 ^ 5` 次。

__思路:__

```text
优先队列
维护一个最小堆
预约时弹出堆顶
释放座位时直接插入堆即可
时间复杂度为 O(N + MlogN), 空间复杂度为 O(N), M 为两个函数的调用次数
```

__代码:__

__C++__:

```C++
class SeatManager 
{
public:
    SeatManager(int n) 
    {
        for (int i = 1; i <= n; i++) seats.insert(i);
    }
    
    int reserve() 
    {
        int available = *seats.begin();
        seats.erase(seats.begin());
        return available;
    }
    
    void unreserve(int seatNumber) 
    {
        seats.insert(seatNumber);
    }
private:
    set<int> seats;
};

/**
 * Your SeatManager object will be instantiated and called as such:
 * SeatManager* obj = new SeatManager(n);
 * int param_1 = obj->reserve();
 * obj->unreserve(seatNumber);
 */
```

__Java__:

```Java
class SeatManager {
    private PriorityQueue<Integer> pq = new PriorityQueue<>();

    public SeatManager(int n) {
        for (int i = 1; i <= n; ++i) pq.offer(i);
    }

    public int reserve() {
        return pq.poll();
    }

    public void unreserve(int seatNumber) {
        pq.offer(seatNumber);
    }
}

/**
 * Your SeatManager object will be instantiated and called as such:
 * SeatManager obj = new SeatManager(n);
 * int param_1 = obj.reserve();
 * obj.unreserve(seatNumber);
 */
```

__Python__:

```Python
class SeatManager:

    def __init__(self, n: int):
        self.seats = list(range(n))
        heapify(self.seats)


    def reserve(self) -> int:
        return heappop(self.seats) + 1


    def unreserve(self, seatNumber: int) -> None:
        heappush(self.seats, seatNumber - 1)



# Your SeatManager object will be instantiated and called as such:
# obj = SeatManager(n)
# param_1 = obj.reserve()
# obj.unreserve(seatNumber)
```
