__Description__:
Given a list of airline tickets represented by pairs of departure and arrival airports [from, to], reconstruct the itinerary in order. All of the tickets belong to a man who departs from JFK. Thus, the itinerary must begin with JFK.

__Note:__

If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string. For example, the itinerary ["JFK", "LGA"] has a smaller lexical order than ["JFK", "LGB"].
All airports are represented by three capital letters (IATA code).
You may assume all tickets form at least one valid itinerary.
One must use all the tickets once and only once.

__Example:__
Example 1:

Input: [["MUC", "LHR"], ["JFK", "MUC"], ["SFO", "SJC"], ["LHR", "SFO"]]
Output: ["JFK", "MUC", "LHR", "SFO", "SJC"]

Example 2:

Input: [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
Output: ["JFK","ATL","JFK","SFO","ATL","SFO"]
Explanation: Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"].
             But it is larger in lexical order.

__题目描述__:
给定一个机票的字符串二维数组 [from, to]，子数组中的两个成员分别表示飞机出发和降落的机场地点，对该行程进行重新规划排序。所有这些机票都属于一个从 JFK（肯尼迪国际机场）出发的先生，所以该行程必须从 JFK 开始。

__提示：__

如果存在多种有效的行程，请你按字符自然排序返回最小的行程组合。例如，行程 ["JFK", "LGA"] 与 ["JFK", "LGB"] 相比就更小，排序更靠前
所有的机场都用三个大写字母表示（机场代码）。
假定所有机票至少存在一种合理的行程。
所有的机票必须都用一次 且 只能用一次。
 
__示例 :__
示例 1：

输入：[["MUC", "LHR"], ["JFK", "MUC"], ["SFO", "SJC"], ["LHR", "SFO"]]
输出：["JFK", "MUC", "LHR", "SFO", "SJC"]

示例 2：

输入：[["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
输出：["JFK","ATL","JFK","SFO","ATL","SFO"]
解释：另一种有效的行程是 ["JFK","SFO","ATL","JFK","ATL","SFO"]。但是它自然排序更大更靠后。

__思路__:
图遍历
保存节点的邻接节点的时候使用优先队列可以保证按序访问
递归结束的条件是从该节点出发能够完全遍历(由题设可知, 一定存在这样的序列), 遍历完加入结果
将结果逆序输出就是行程
时间复杂度O(elge), 空间复杂度O(e), 其中 e为图中的边数

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<string> findItinerary(vector<vector<string>>& tickets) 
    {
        for (auto& ticket : tickets) graph[ticket[0]].emplace(ticket[1]);
        helper("JFK");
        reverse(result.begin(), result.end());
        return result;
    }
private:
    unordered_map<string, priority_queue<string, vector<string>, std::greater<string>>> graph;
    
    vector<string> result;
    
    void helper(const string& start) 
    {
        while (graph.count(start) && graph[start].size() > 0) 
        {
            string destination = graph[start].top();
            graph[start].pop();
            helper(move(destination));
        }
        result.emplace_back(start);
    }
};
```

__Java__:
```Java
class Solution {
    public List<String> findItinerary(List<List<String>> tickets) {
        Map<String, PriorityQueue<String>> map = new HashMap<>();
        List<String> result = new ArrayList<>();
        for (List<String> ticket : tickets) {
            if (!map.containsKey(ticket.get(0))) map.put(ticket.get(0), new PriorityQueue<String>());
            map.get(ticket.get(0)).add(ticket.get(1));
        }
        helper(result, map, "JFK");
        return result;
    }
    
    private void helper(List<String> result, Map<String, PriorityQueue<String>> map, String start) {
        PriorityQueue<String> pq = map.get(start);
        while (pq != null && !pq.isEmpty()) helper(result, map, pq.poll());
        result.add(0, start);
    }
}
```

__Python__:
```Python
class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        graph, result = collections.defaultdict(list), []
        for start, target in tickets:
            graph[start].append(target)
        for start in graph:
            graph[start].sort(reverse=True)
        def helper(start):
            while graph[start]:
                helper(graph[start].pop())
            result.append(start)
        helper("JFK")
        return result[::-1]
```