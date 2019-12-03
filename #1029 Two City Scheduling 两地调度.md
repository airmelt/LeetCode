__Description__:
There are 2N people a company is planning to interview. The cost of flying the i-th person to city A is costs[i][0], and the cost of flying the i-th person to city B is costs[i][1].

Return the minimum cost to fly every person to a city such that exactly N people arrive in each city.

__Example:__
Example 1:

Input: [[10,20],[30,200],[400,50],[30,20]]
Output: 110
Explanation: 
The first person goes to city A for a cost of 10.
The second person goes to city A for a cost of 30.
The third person goes to city B for a cost of 50.
The fourth person goes to city B for a cost of 20.

The total minimum cost is 10 + 30 + 50 + 20 = 110 to have half the people interviewing in each city.
 
__Note:__

1 <= costs.length <= 100
It is guaranteed that costs.length is even.
1 <= costs[i][0], costs[i][1] <= 1000

__题目描述__:
公司计划面试 2N 人。第 i 人飞往 A 市的费用为 costs[i][0]，飞往 B 市的费用为 costs[i][1]。

返回将每个人都飞到某座城市的最低费用，要求每个城市都有 N 人抵达。

__示例 :__
输入：[[10,20],[30,200],[400,50],[30,20]]
输出：110
解释：
第一个人去 A 市，费用为 10。
第二个人去 A 市，费用为 30。
第三个人去 B 市，费用为 50。
第四个人去 B 市，费用为 20。

最低总费用为 10 + 30 + 50 + 20 = 110，每个城市都有一半的人在面试。
 
__提示：__

1 <= costs.length <= 100
costs.length 为偶数
1 <= costs[i][0], costs[i][1] <= 1000

__思路__:
假设公司把所有人都派去 B地, 再从其中选出一半的人去 A地, 那么去 A地的人的额外支出就是 costs[i][0] - costs[i][1], 只要按 costs[i][0] - costs[i][1]排序, 前一半的人去 A地后一半的人去 B地即可
时间复杂度O(nlgn), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int twoCitySchedCost(vector<vector<int>>& costs) 
    {
        sort(costs.begin(), costs.end(), [](const vector<int> &c1, const vector<int> &c2) { return (c1[0] - c1[1] < c2[0] - c2[1]); });
        int result = 0, n = costs.size() / 2;
        for (int i = 0; i < n; i++) result += costs[i][0] + costs[i + n][1];
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int twoCitySchedCost(int[][] costs) {
        Arrays.sort(costs, new Comparator<int[]>() {
            @Override
            public int compare(int[] c1, int[] c2) {
              return c1[0] - c1[1] - (c2[0] - c2[1]);
            }
        });
        int result = 0, n = costs.length / 2;
        for (int i = 0; i < n; i++) result += costs[i][0] + costs[i + n][1];
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def twoCitySchedCost(self, costs: List[List[int]]) -> int:
        return sum([p[0] for i, p in enumerate(sorted(costs, key=lambda x: x[0] - x[1])) if i < len(costs) // 2]) + sum([p[1] for i, p in enumerate(sorted(costs, key=lambda x: x[0] - x[1])) if i >= len(costs) // 2])
```