# 1282 Group the People Given the Group Size They Belong To 用户分组

__Description:__

There are n people that are split into some unknown number of groups. Each person is labeled with a unique ID from 0 to n - 1.

You are given an integer array groupSizes, where groupSizes[i] is the size of the group that person i is in. For example, if groupSizes[1] = 3, then person 1 must be in a group of size 3.

Return a list of groups such that each person i is in a group of size groupSizes[i].

Each person should appear in exactly one group, and every person must be in a group. If there are multiple answers, return any of them. It is guaranteed that there will be at least one valid solution for the given input.

__Example:__

Example 1:

Input: groupSizes = [3,3,3,3,3,1,3]
Output: [[5],[0,1,2],[3,4,6]]
Explanation:
The first group is [5]. The size is 1, and groupSizes[5] = 1.
The second group is [0,1,2]. The size is 3, and groupSizes[0] = groupSizes[1] = groupSizes[2] = 3.
The third group is [3,4,6]. The size is 3, and groupSizes[3] = groupSizes[4] = groupSizes[6] = 3.
Other possible solutions are [[2,1,6],[5],[0,4,3]] and [[5],[0,6,2],[4,3,1]].

Example 2:

Input: groupSizes = [2,1,3,3,3,2]
Output: [[1],[0,5],[2,3,4]]

__Constraints:__

groupSizes.length == n
1 <= n <= 500
1 <= groupSizes[i] <= n

__题目描述：__

有 n 个人被分成数量未知的组。每个人都被标记为一个从 0 到 n - 1 的唯一ID 。

给定一个整数数组 groupSizes ，其中 groupSizes[i] 是第 i 个人所在的组的大小。例如，如果 groupSizes[1] = 3 ，则第 1 个人必须位于大小为 3 的组中。

返回一个组列表，使每个人 i 都在一个大小为 groupSizes[i] 的组中。

每个人应该 恰好只 出现在 一个组 中，并且每个人必须在一个组中。如果有多个答案，返回其中 任何 一个。可以 保证 给定输入 至少有一个 有效的解。

__示例：__

示例 1：

输入：groupSizes = [3,3,3,3,3,1,3]
输出：[[5],[0,1,2],[3,4,6]]
解释：
第一组是 [5]，大小为 1，groupSizes[5] = 1。
第二组是 [0,1,2]，大小为 3，groupSizes[0] = groupSizes[1] = groupSizes[2] = 3。
第三组是 [3,4,6]，大小为 3，groupSizes[3] = groupSizes[4] = groupSizes[6] = 3。
其他可能的解决方案有 [[2,1,6],[5],[0,4,3]] 和 [[5],[0,6,2],[4,3,1]]。

示例 2：

输入：groupSizes = [2,1,3,3,3,2]
输出：[[1],[0,5],[2,3,4]]

__提示：__

groupSizes.length == n
1 <= n <= 500
1 <= groupSizes[i] <= n

__思路：__

栈
用一个栈 left 记录出现的左括号的位置
每次遇到右括号, 如果栈非空就弹出
否则加入到哈希表中
最后栈中的元素都加入哈希表中, 哈希表中都是需要删除的不匹配括号的下标
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> groupThePeople(vector<int>& groupSizes) 
    {
        unordered_map<int, vector<int>> group;
        vector<vector<int>> result;
        for (int i = 0, n = groupSizes.size(); i < n; i++) group[groupSizes[i]].emplace_back(i);
        for (const auto& it : group) for (auto cur = it.second.begin(); cur != it.second.end(); cur += it.first) result.emplace_back(vector<int>(cur, cur + it.first));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> groupThePeople(int[] groupSizes) {
        Map<Integer, List<Integer>> map = new HashMap<>();
        List<List<Integer>> result = new ArrayList<>();
        for (int i = 0, n = groupSizes.length; i < n; i++) {
            if (!map.containsKey(groupSizes[i])) map.put(groupSizes[i], new ArrayList<>());
            map.get(groupSizes[i]).add(i);
        }
        for (Map.Entry<Integer, List<Integer>> entry : map.entrySet()) {
            for (int i = 0, groupSize = entry.getKey(), n = entry.getValue().size() / groupSize; i < n; i++) {
                List<Integer> group = new ArrayList<>();
                for (int j = 0; j < groupSize; j++) group.add(entry.getValue().get(i * groupSize + j));
                result.add(group);
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def groupThePeople(self, groupSizes: List[int]) -> List[List[int]]:
        d = defaultdict(list)
        for i, v in enumerate(groupSizes):
            d[v].append(i)
        return [v[i * k:i * k + k] for k, v in d.items() for i in range(len(v) // k)]
```
