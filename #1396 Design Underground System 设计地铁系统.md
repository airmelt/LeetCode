# 1396 Design Underground System 设计地铁系统

__Description:__

An underground railway system is keeping track of customer travel times between different stations. They are using this data to calculate the average time it takes to travel from one station to another.

Implement the UndergroundSystem class:

void checkIn(int id, string stationName, int t)
A customer with a card ID equal to id, checks in at the station stationName at time t.
A customer can only be checked into one place at a time.
void checkOut(int id, string stationName, int t)
A customer with a card ID equal to id, checks out from the station stationName at time t.
double getAverageTime(string startStation, string endStation)
Returns the average time it takes to travel from startStation to endStation.
The average time is computed from all the previous traveling times from startStation to endStation that happened directly, meaning a check in at startStation followed by a check out from endStation.
The time it takes to travel from startStation to endStation may be different from the time it takes to travel from endStation to startStation.
There will be at least one customer that has traveled from startStation to endStation before getAverageTime is called.

A customer with a card ID equal to id, checks in at the station stationName at time t.
A customer can only be checked into one place at a time.

A customer with a card ID equal to id, checks out from the station stationName at time t.

Returns the average time it takes to travel from startStation to endStation.
The average time is computed from all the previous traveling times from startStation to endStation that happened directly, meaning a check in at startStation followed by a check out from endStation.
The time it takes to travel from startStation to endStation may be different from the time it takes to travel from endStation to startStation.
There will be at least one customer that has traveled from startStation to endStation before getAverageTime is called.

You may assume all calls to the checkIn and checkOut methods are consistent. If a customer checks in at time t1 then checks out at time t2, then t1 < t2. All events happen in chronological order.

__Example:__

Example 1:

Input
["UndergroundSystem","checkIn","checkIn","checkIn","checkOut","checkOut","checkOut","getAverageTime","getAverageTime","checkIn","getAverageTime","checkOut","getAverageTime"]
[[],[45,"Leyton",3],[32,"Paradise",8],[27,"Leyton",10],[45,"Waterloo",15],[27,"Waterloo",20],[32,"Cambridge",22],["Paradise","Cambridge"],["Leyton","Waterloo"],[10,"Leyton",24],["Leyton","Waterloo"],[10,"Waterloo",38],["Leyton","Waterloo"]]

Output
[null,null,null,null,null,null,null,14.00000,11.00000,null,11.00000,null,12.00000]

Explanation

```Java
UndergroundSystem undergroundSystem = new UndergroundSystem();
undergroundSystem.checkIn(45, "Leyton", 3);
undergroundSystem.checkIn(32, "Paradise", 8);
undergroundSystem.checkIn(27, "Leyton", 10);
undergroundSystem.checkOut(45, "Waterloo", 15);  // Customer 45 "Leyton" -> "Waterloo" in 15-3 = 12
undergroundSystem.checkOut(27, "Waterloo", 20);  // Customer 27 "Leyton" -> "Waterloo" in 20-10 = 10
undergroundSystem.checkOut(32, "Cambridge", 22); // Customer 32 "Paradise" -> "Cambridge" in 22-8 = 14
undergroundSystem.getAverageTime("Paradise", "Cambridge"); // return 14.00000. One trip "Paradise" -> "Cambridge", (14) / 1 = 14
undergroundSystem.getAverageTime("Leyton", "Waterloo");    // return 11.00000. Two trips "Leyton" -> "Waterloo", (10 + 12) / 2 = 11
undergroundSystem.checkIn(10, "Leyton", 24);
undergroundSystem.getAverageTime("Leyton", "Waterloo");    // return 11.00000
undergroundSystem.checkOut(10, "Waterloo", 38);  // Customer 10 "Leyton" -> "Waterloo" in 38-24 = 14
undergroundSystem.getAverageTime("Leyton", "Waterloo");    // return 12.00000. Three trips "Leyton" -> "Waterloo", (10 + 12 + 14) / 3 = 12
```

Example 2:

Input
["UndergroundSystem","checkIn","checkOut","getAverageTime","checkIn","checkOut","getAverageTime","checkIn","checkOut","getAverageTime"]
[[],[10,"Leyton",3],[10,"Paradise",8],["Leyton","Paradise"],[5,"Leyton",10],[5,"Paradise",16],["Leyton","Paradise"],[2,"Leyton",21],[2,"Paradise",30],["Leyton","Paradise"]]

Output
[null,null,null,5.00000,null,null,5.50000,null,null,6.66667]

Explanation

```Java
UndergroundSystem undergroundSystem = new UndergroundSystem();
undergroundSystem.checkIn(10, "Leyton", 3);
undergroundSystem.checkOut(10, "Paradise", 8); // Customer 10 "Leyton" -> "Paradise" in 8-3 = 5
undergroundSystem.getAverageTime("Leyton", "Paradise"); // return 5.00000, (5) / 1 = 5
undergroundSystem.checkIn(5, "Leyton", 10);
undergroundSystem.checkOut(5, "Paradise", 16); // Customer 5 "Leyton" -> "Paradise" in 16-10 = 6
undergroundSystem.getAverageTime("Leyton", "Paradise"); // return 5.50000, (5 + 6) / 2 = 5.5
undergroundSystem.checkIn(2, "Leyton", 21);
undergroundSystem.checkOut(2, "Paradise", 30); // Customer 2 "Leyton" -> "Paradise" in 30-21 = 9
undergroundSystem.getAverageTime("Leyton", "Paradise"); // return 6.66667, (5 + 6 + 9) / 3 = 6.66667
```

__Constraints:__

1 <= id, t <= 10^6
1 <= stationName.length, startStation.length, endStation.length <= 10
All strings consist of uppercase and lowercase English letters and digits.
There will be at most 2 * 10^4 calls in total to checkIn, checkOut, and getAverageTime.
Answers within 10^-5 of the actual value will be accepted.

__题目描述:__

地铁系统跟踪不同车站之间的乘客出行时间，并使用这一数据来计算从一站到另一站的平均时间。

实现 UndergroundSystem 类：

void checkIn(int id, string stationName, int t)
通行卡 ID 等于 id 的乘客，在时间 t ，从 stationName 站进入
乘客一次只能从一个站进入
void checkOut(int id, string stationName, int t)
通行卡 ID 等于 id 的乘客，在时间 t ，从 stationName 站离开
double getAverageTime(string startStation, string endStation)
返回从 startStation 站到 endStation 站的平均时间
平均时间会根据截至目前所有从 startStation 站 直接 到达 endStation 站的行程进行计算，也就是从 startStation 站进入并从 endStation 离开的行程
从 startStation 到 endStation 的行程时间与从 endStation 到 startStation 的行程时间可能不同
在调用 getAverageTime 之前，至少有一名乘客从 startStation 站到达 endStation 站

通行卡 ID 等于 id 的乘客，在时间 t ，从 stationName 站进入
乘客一次只能从一个站进入

通行卡 ID 等于 id 的乘客，在时间 t ，从 stationName 站离开

返回从 startStation 站到 endStation 站的平均时间
平均时间会根据截至目前所有从 startStation 站 直接 到达 endStation 站的行程进行计算，也就是从 startStation 站进入并从 endStation 离开的行程
从 startStation 到 endStation 的行程时间与从 endStation 到 startStation 的行程时间可能不同
在调用 getAverageTime 之前，至少有一名乘客从 startStation 站到达 endStation 站

你可以假设对 checkIn 和 checkOut 方法的所有调用都是符合逻辑的。如果一名乘客在时间 t1 进站、时间 t2 出站，那么 t1 < t2 。所有时间都按时间顺序发生。

__示例:__

示例 1：

输入
["UndergroundSystem","checkIn","checkIn","checkIn","checkOut","checkOut","checkOut","getAverageTime","getAverageTime","checkIn","getAverageTime","checkOut","getAverageTime"]
[[],[45,"Leyton",3],[32,"Paradise",8],[27,"Leyton",10],[45,"Waterloo",15],[27,"Waterloo",20],[32,"Cambridge",22],["Paradise","Cambridge"],["Leyton","Waterloo"],[10,"Leyton",24],["Leyton","Waterloo"],[10,"Waterloo",38],["Leyton","Waterloo"]]

输出
[null,null,null,null,null,null,null,14.00000,11.00000,null,11.00000,null,12.00000]

解释

```Java
UndergroundSystem undergroundSystem = new UndergroundSystem();
undergroundSystem.checkIn(45, "Leyton", 3);
undergroundSystem.checkIn(32, "Paradise", 8);
undergroundSystem.checkIn(27, "Leyton", 10);
undergroundSystem.checkOut(45, "Waterloo", 15);  // 乘客 45 "Leyton" -> "Waterloo" ，用时 15-3 = 12
undergroundSystem.checkOut(27, "Waterloo", 20);  // 乘客 27 "Leyton" -> "Waterloo" ，用时 20-10 = 10
undergroundSystem.checkOut(32, "Cambridge", 22); // 乘客 32 "Paradise" -> "Cambridge" ，用时 22-8 = 14
undergroundSystem.getAverageTime("Paradise", "Cambridge"); // 返回 14.00000 。只有一个 "Paradise" -> "Cambridge" 的行程，(14) / 1 = 14
undergroundSystem.getAverageTime("Leyton", "Waterloo");    // 返回 11.00000 。有两个 "Leyton" -> "Waterloo" 的行程，(10 + 12) / 2 = 11
undergroundSystem.checkIn(10, "Leyton", 24);
undergroundSystem.getAverageTime("Leyton", "Waterloo");    // 返回 11.00000
undergroundSystem.checkOut(10, "Waterloo", 38);  // 乘客 10 "Leyton" -> "Waterloo" ，用时 38-24 = 14
undergroundSystem.getAverageTime("Leyton", "Waterloo");    // 返回 12.00000 。有三个 "Leyton" -> "Waterloo" 的行程，(10 + 12 + 14) / 3 = 12
```

示例 2：

输入
["UndergroundSystem","checkIn","checkOut","getAverageTime","checkIn","checkOut","getAverageTime","checkIn","checkOut","getAverageTime"]
[[],[10,"Leyton",3],[10,"Paradise",8],["Leyton","Paradise"],[5,"Leyton",10],[5,"Paradise",16],["Leyton","Paradise"],[2,"Leyton",21],[2,"Paradise",30],["Leyton","Paradise"]]

输出
[null,null,null,5.00000,null,null,5.50000,null,null,6.66667]

解释

```Java
UndergroundSystem undergroundSystem = new UndergroundSystem();
undergroundSystem.checkIn(10, "Leyton", 3);
undergroundSystem.checkOut(10, "Paradise", 8); // 乘客 10 "Leyton" -> "Paradise" ，用时 8-3 = 5
undergroundSystem.getAverageTime("Leyton", "Paradise"); // 返回 5.00000 ，(5) / 1 = 5
undergroundSystem.checkIn(5, "Leyton", 10);
undergroundSystem.checkOut(5, "Paradise", 16); // 乘客 5 "Leyton" -> "Paradise" ，用时 16-10 = 6
undergroundSystem.getAverageTime("Leyton", "Paradise"); // 返回 5.50000 ，(5 + 6) / 2 = 5.5
undergroundSystem.checkIn(2, "Leyton", 21);
undergroundSystem.checkOut(2, "Paradise", 30); // 乘客 2 "Leyton" -> "Paradise" ，用时 30-21 = 9
undergroundSystem.getAverageTime("Leyton", "Paradise"); // 返回 6.66667 ，(5 + 6 + 9) / 3 = 6.66667
```

__提示：__

1 <= id, t <= 10^6
1 <= stationName.length, startStation.length, endStation.length <= 10 次
所有字符串由大小写英文字母与数字组成
总共最多调用 checkIn、checkOut 和 getAverageTime 方法 2 * 10^4
与标准答案误差在 10^-5 以内的结果都被视为正确结果

__思路:__

```text
模拟
使用两个哈希表
一个表记录进站的 id, 站名和进站时间
另一个表记录进站和离站的站名, 总共消耗的时间以及人数
如果有人下车, 更新下车表中的站的消耗时间, 并将该站人数加 1
计算时间直接用下车表中的时间除以人数即可
时间复杂度为 O(1), 空间复杂度为 O(N ^ 2), 其中 N 表示操作次数, 对应每个记录分别有入站和出站的信息需要记录
```

__代码:__

__C++__:

```C++
class UndergroundSystem 
{
private:
    unordered_map<int, pair<string, int>> station_info;
    unordered_map<string, pair<double, int>> leave_info;
public:
    UndergroundSystem() {}
    
    void checkIn(int id, string stationName, int t)
    {
        station_info[id] = make_pair(stationName, t);
    }
    
    void checkOut(int id, string stationName, int t) 
    {
        const auto &[start_station, start_time] = station_info[id];
        const auto [total_time, station_count] = leave_info[start_station + ',' + stationName];
        leave_info[start_station + ',' + stationName] = make_pair(total_time + t - start_time, station_count + 1);
    }
    
    double getAverageTime(string startStation, string endStation)
    {
        return leave_info[startStation + ',' + endStation].first / leave_info[startStation + ',' + endStation].second;
    }
};

/**
 * Your UndergroundSystem object will be instantiated and called as such:
 * UndergroundSystem* obj = new UndergroundSystem();
 * obj->checkIn(id,stationName,t);
 * obj->checkOut(id,stationName,t);
 * double param_3 = obj->getAverageTime(startStation,endStation);
 */
```

__Java__:

```Java
class UndergroundSystem {
    private Map<Integer, String> stationInfo = new HashMap<>();
    private Map<String, List<Double>> leaveInfo = new HashMap<>();

    public UndergroundSystem() {

    }
    
    public void checkIn(int id, String stationName, int t) {
        stationInfo.put(id, stationName + "_" + String.valueOf(t));
    }
    
    public void checkOut(int id, String stationName, int t) {
        String value = stationInfo.get(id), station = value.substring(0, value.indexOf("_")) + "_" + stationName;
        int startTime = Integer.parseInt(value.substring(value.indexOf("_") + 1, value.length()));
        if (leaveInfo.containsKey(station)) {
            List<Double> list = leaveInfo.get(station);
            double totalTime = list.get(list.size() - 1);
            list.add(totalTime + t - startTime);
            leaveInfo.put(station, list);
        } else {
            List<Double> list = new ArrayList<>();
            list.add(1.0 * t - startTime);
            leaveInfo.put(station, list);
        }
    }
    
    public double getAverageTime(String startStation, String endStation) {
        return leaveInfo.get(startStation + "_" + endStation).get(leaveInfo.get(startStation + "_" + endStation).size() - 1) / leaveInfo.get(startStation + "_" + endStation).size();
    }
}

/**
 * Your UndergroundSystem object will be instantiated and called as such:
 * UndergroundSystem obj = new UndergroundSystem();
 * obj.checkIn(id,stationName,t);
 * obj.checkOut(id,stationName,t);
 * double param_3 = obj.getAverageTime(startStation,endStation);
 */
```

__Python__:

```Python
class UndergroundSystem:

    def __init__(self):
        self.station_info = defaultdict()
        self.leave_info = defaultdict(lambda: (0.0, 0))


    def checkIn(self, id: int, stationName: str, t: int) -> None:
        self.station_info[id] = (stationName, t)


    def checkOut(self, id: int, stationName: str, t: int) -> None:
        
        self.leave_info[station] = (self.leave_info[station := self.station_info[id][0] + ',' + stationName][0] + t - self.station_info[id][1], self.leave_info[station][1] + 1)


    def getAverageTime(self, startStation: str, endStation: str) -> float:
        return self.leave_info[station := startStation + ',' + endStation][0] / self.leave_info[station][1]



# Your UndergroundSystem object will be instantiated and called as such:
# obj = UndergroundSystem()
# obj.checkIn(id,stationName,t)
# obj.checkOut(id,stationName,t)
# param_3 = obj.getAverageTime(startStation,endStation)
```
