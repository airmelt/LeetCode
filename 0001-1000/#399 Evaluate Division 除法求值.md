# 399 Evaluate Division 除法求值

__Description__:
You are given an array of variable pairs equations and an array of real numbers values, where equations[i] = [Ai, Bi] and values[i] represent the equation Ai / Bi = values[i]. Each Ai or Bi is a string that represents a single variable.

You are also given some queries, where queries[j] = [Cj, Dj] represents the jth query where you must find the answer for Cj / Dj = ?.

Return the answers to all queries. If a single answer cannot be determined, return -1.0.

__Note:__
The input is always valid. You may assume that evaluating the queries will not result in division by zero and that there is no contradiction.

__Example:__

Example 1:

Input: equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
Output: [6.00000,0.50000,-1.00000,1.00000,-1.00000]
Explanation:
Given: a / b = 2.0, b / c = 3.0
queries are: a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ?
return: [6.0, 0.5, -1.0, 1.0, -1.0 ]

Example 2:

Input: equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
Output: [3.75000,0.40000,5.00000,0.20000]

Example 3:

Input: equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
Output: [0.50000,2.00000,-1.00000,-1.00000]

__Constraints:__

1 <= equations.length <= 20
equations[i].length == 2
1 <= Ai.length, Bi.length <= 5
values.length == equations.length
0.0 < values[i] <= 20.0
1 <= queries.length <= 20
queries[i].length == 2
1 <= Cj.length, Dj.length <= 5
Ai, Bi, Cj, Dj consist of lower case English letters and digits.

__题目描述__:
给出方程式 A / B = k, 其中 A 和 B 均为用字符串表示的变量， k 是一个浮点型数字。根据已知方程式求解问题，并返回计算结果。如果结果不存在，则返回 -1.0。

输入总是有效的。你可以假设除法运算中不会出现除数为 0 的情况，且不存在任何矛盾的结果。

__示例 :__

示例 1：

输入：equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
输出：[6.00000,0.50000,-1.00000,1.00000,-1.00000]
解释：
给定：a / b = 2.0, b / c = 3.0
问题：a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ?
返回：[6.0, 0.5, -1.0, 1.0, -1.0 ]

示例 2：

输入：equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
输出：[3.75000,0.40000,5.00000,0.20000]

示例 3：

输入：equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
输出：[0.50000,2.00000,-1.00000,-1.00000]

__提示：__

1 <= equations.length <= 20
equations[i].length == 2
1 <= equations[i][0].length, equations[i][1].length <= 5
values.length == equations.length
0.0 < values[i] <= 20.0
1 <= queries.length <= 20
queries[i].length == 2
1 <= queries[i][0].length, queries[i][1].length <= 5
equations[i][0], equations[i][1], queries[i][0], queries[i][1] 由小写英文字母与数字组成

__思路__:

转化为图是否有路径
用 bfs或者 dfs查找路径, 注意剪枝加快查找速度
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    unordered_map<string, unordered_set<string>> graph;
    unordered_map<string, double> weight;
    vector<double> result;
    
    double dfs(const string &start, const string &end, unordered_set<string> &visited)
    {
        if (!graph.count(start) or !graph.count(end)) return 0;
        if (weight.count(start + "#" + end)) return weight[start + "#" + end];
        if (start == end) return 1.0;
        if (visited.count(start)) return 0;
        visited.insert(start);
        double result = 0;
        for (auto &edge : graph[start]) 
        {
            result = dfs(edge, end, visited) * weight[start + "#" + edge];
            if (result) 
            {
                weight[start + "#" + end] = result;
                break;
            }
        }
        visited.erase(start);
        return result;
    }
public:
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) 
    {
        result.resize(queries.size());
        for (int i = 0; i < equations.size() ;i++)
        {
            graph[equations[i].front()].insert(equations[i].back());
            graph[equations[i].back()].insert(equations[i].front());
            weight[equations[i].front() + "#" + equations[i].back()] = values[i];
            weight[equations[i].back() + "#" + equations[i].front()] = 1.0 / values[i];
        }
        for (int i = 0; i < queries.size(); i++) 
        {
            unordered_set<string> visited;
            double temp = dfs(queries[i].front(), queries[i].back(), visited);
            if (!temp) temp = -1.0;
            result[i] = temp;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private Map<String, Set<String>> graph = new HashMap<>();
    private Map<String, Double> weight = new HashMap<>();
    
    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
        double result[] = new double[queries.size()];
        for (int i = 0; i < values.length; i++) {
            Set<String> set = graph.getOrDefault(equations.get(i).get(0), new HashSet<String>());
            set.add(equations.get(i).get(1));
            graph.put(equations.get(i).get(0), set);
            set = graph.getOrDefault(equations.get(i).get(1), new HashSet<String>()); 
            set.add(equations.get(i).get(0));
            graph.put(equations.get(i).get(1), set);
            weight.put(equations.get(i).get(0) + "#" + equations.get(i).get(1), values[i]);
            weight.put(equations.get(i).get(1) + "#" + equations.get(i).get(0), 1.0 / values[i]);
        }
        for (int i = 0; i < queries.size(); i++) {
            double temp = dfs(queries.get(i).get(0), queries.get(i).get(1), new HashSet<String>());
            if (temp == 0) temp = -1.0;
            result[i] = temp;
        }
        return result;
    }
            
    private double dfs(String start, String end, Set<String> visited) {
            if (weight.containsKey(start + "#" + end)) return weight.get(start + "#" + end);
            if (!graph.containsKey(start) || !graph.containsKey(end)) return 0;
            if (visited.contains(start)) return 0;
            visited.add(start);
            double result = 0;
            for (String edge : graph.get(start)) {
                result = dfs(edge, end, visited) * weight.get(start + "#" + edge);
                if (result != 0) {
                    weight.put(start + "#" + end, result);
                    break;
                }
            }
            visited.remove(start);
            return result;
    }
}
```

__Python__:

```Python
class Solution:
    def calcEquation(self, equations: List[List[str]], values: List[float], queries: List[List[str]]) -> List[float]:
        graph, weight, result = collections.defaultdict(set), collections.defaultdict(), []
        for i, edges in enumerate(equations):
            graph[edges[0]].add(edges[1])
            graph[edges[1]].add(edges[0])
            weight[tuple(edges)], weight[tuple(edges[::-1])] = values[i], 1.0 / values[i]
        def dfs(start: str, end: str, visited: Set) -> float:
            if (start, end) in weight:
                return weight[(start, end)]
            if start not in graph or end not in graph:
                return 0
            if start == end:
                return 1
            if start in visited:
                return 0
            visited.add(start)
            result = 0
            for edge in graph[start]:
                result = dfs(edge, end, visited) * weight[(start, edge)]
                if result:
                    weight[(start, end)] = result
                    break
            visited.remove(start)
            return result
        for query in queries:
            temp = dfs(query[0], query[1], set())
            if not temp:
                temp = -1.0
            result.append(temp)
        return result
```
