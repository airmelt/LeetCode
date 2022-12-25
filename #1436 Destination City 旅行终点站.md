# 1436 Destination City 旅行终点站

__Description:__

You are given the array `paths`, where `paths[i] = [cityAi, cityBi]` means there exists a direct path going from `cityAi` to `cityBi`. _Return the destination city, that is, the city without any path outgoing to another city._

It is guaranteed that the graph of paths forms a line without any loop, therefore, there will be exactly one destination city.

__Example:__

Example 1:

```text
Input: paths = [["London","New York"],["New York","Lima"],["Lima","Sao Paulo"]]
Output: "Sao Paulo" 
Explanation: Starting at "London" city you will reach "Sao Paulo" city which is the destination city. Your trip consist of: "London" -> "New York" -> "Lima" -> "Sao Paulo".
```

Example 2:

```text
Input: paths = [["B","C"],["D","B"],["C","A"]]
Output: "A"
Explanation: All possible trips are: 
"D" -> "B" -> "C" -> "A". 
"B" -> "C" -> "A". 
"C" -> "A". 
"A". 
Clearly the destination city is "A".
```

Example 3:

```text
Input: paths = [["A","Z"]]
Output: "Z"
```

__Constraints:__

- `1 <= paths.length <= 100`
- `paths[i].length == 2`
- `1 <= cityAi.length, cityBi.length <= 10`
- `cityAi != cityBi`
- All strings consist of lowercase and uppercase English letters and the space character.

__题目描述:__

给你一份旅游线路图，该线路图中的旅行线路用数组 `paths` 表示，其中 `paths[i] = [cityAi, cityBi]` 表示该线路将会从 `cityAi` 直接前往 `cityBi` 。请你找出这次旅行的终点站，即没有任何可以通往其他城市的线路的城市_。_

题目数据保证线路图会形成一条不存在循环的线路，因此恰有一个旅行终点站。

__示例:__

示例 1：

```text
输入：paths = [["London","New York"],["New York","Lima"],["Lima","Sao Paulo"]]
输出："Sao Paulo" 
解释：从 "London" 出发，最后抵达终点站 "Sao Paulo" 。本次旅行的路线是 "London" -> "New York" -> "Lima" -> "Sao Paulo" 。
```

示例 2：

```text
输入：paths = [["B","C"],["D","B"],["C","A"]]
输出："A"
解释：所有可能的线路是：
"D" -> "B" -> "C" -> "A". 
"B" -> "C" -> "A". 
"C" -> "A". 
"A". 
显然，旅行终点站是 "A" 。
```

示例 3：

```text
输入：paths = [["A","Z"]]
输出："Z"
```

__提示：__

- `1 <= paths.length <= 100`
- `paths[i].length == 2`
- `1 <= cityAi.length, cityBi.length <= 10`
- `cityAi != cityBi`
- 所有字符串均由大小写英文字母和空格字符组成。

__思路:__

```text
哈希表
因为终点必有一个
将所有起点加入一个哈希表
终点中不存在于起点哈希表的就是旅行终点站
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string destCity(vector<vector<string>>& paths) 
    {
        unordered_set<string> s;
        for (const auto& path: paths) s.insert(path.front());
        for (const auto& path: paths) if (!s.count(path.back())) return path.back();
        return "";
    }
};
```

__Java__:

```Java
class Solution {
    public String destCity(List<List<String>> paths) {
        Set<String> set = new HashSet<>();
        for (List<String> path: paths) set.add(path.get(0));
        for (List<String> path: paths) if (!set.contains(path.get(1))) return path.get(1);
        return "";
    }
}
```

__Python__:

```Python
class Solution:
    def destCity(self, paths: List[List[str]]) -> str:
        return (set(end for _, end in paths) - set(start for start, _ in paths)).pop()
```
