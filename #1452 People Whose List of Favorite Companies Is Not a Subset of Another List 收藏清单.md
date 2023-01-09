# 1452 People Whose List of Favorite Companies Is Not a Subset of Another List 收藏清单

__Description:__

Given the array  `favoriteCompanies` where  `favoriteCompanies[i]` is the list of favorites companies for the  `ith` person (__indexed from 0__).

Return the indices of people whose list of favorite companies is not a subset of any other list of favorites companies. You must return the indices in increasing order.

__Example:__

Example 1:

```text
Input: favoriteCompanies = [["leetcode","google","facebook"],["google","microsoft"],["google","facebook"],["google"],["amazon"]]
Output: [0,1,4] 
Explanation: 
Person with index=2 has favoriteCompanies[2]=["google","facebook"] which is a subset of favoriteCompanies[0]=["leetcode","google","facebook"] corresponding to the person with index 0. 
Person with index=3 has favoriteCompanies[3]=["google"] which is a subset of favoriteCompanies[0]=["leetcode","google","facebook"] and favoriteCompanies[1]=["google","microsoft"]. 
Other lists of favorite companies are not a subset of another list, therefore, the answer is [0,1,4].
```

Example 2:

```text
Input: favoriteCompanies = [["leetcode","google","facebook"],["leetcode","amazon"],["facebook","google"]]
Output: [0,1] 
Explanation: In this case favoriteCompanies[2]=["facebook","google"] is a subset of favoriteCompanies[0]=["leetcode","google","facebook"], therefore, the answer is [0,1].
```

Example 3:

```text
Input: favoriteCompanies = [["leetcode"],["google"],["facebook"],["amazon"]]
Output: [0,1,2,3]
```

__Constraints:__

- `1 <= favoriteCompanies.length <= 100`
- `1 <= favoriteCompanies[i].length <= 500`
- `1 <= favoriteCompanies[i][j].length <= 20`
- All strings in  `favoriteCompanies[i]` are __distinct__.
- All lists of favorite companies are __distinct__, that is, If we sort alphabetically each list then  `favoriteCompanies[i] != favoriteCompanies[j].`
- All strings consist of lowercase English letters only.

__题目描述:__

给你一个数组  `favoriteCompanies` ，其中  `favoriteCompanies[i]` 是第  `i` 名用户收藏的公司清单（__下标从 0 开始__）。

请找出不是其他任何人收藏的公司清单的子集的收藏清单，并返回该清单下标。下标需要按升序排列。

__示例:__

示例 1：

```text
输入：favoriteCompanies = [["leetcode","google","facebook"],["google","microsoft"],["google","facebook"],["google"],["amazon"]]
输出：[0,1,4] 
解释：
favoriteCompanies[2]=["google","facebook"] 是 favoriteCompanies[0]=["leetcode","google","facebook"] 的子集。
favoriteCompanies[3]=["google"] 是 favoriteCompanies[0]=["leetcode","google","facebook"] 和 favoriteCompanies[1]=["google","microsoft"] 的子集。
其余的收藏清单均不是其他任何人收藏的公司清单的子集，因此，答案为 [0,1,4] 。
```

示例 2：

```text
输入：favoriteCompanies = [["leetcode","google","facebook"],["leetcode","amazon"],["facebook","google"]]
输出：[0,1] 
解释：favoriteCompanies[2]=["facebook","google"] 是 favoriteCompanies[0]=["leetcode","google","facebook"] 的子集，因此，答案为 [0,1] 。
```

示例 3：

```text
输入：favoriteCompanies = [["leetcode"],["google"],["facebook"],["amazon"]]
输出：[0,1,2,3]
```

__提示：__

- `1 <= favoriteCompanies.length <= 100`
- `1 <= favoriteCompanies[i].length <= 500`
- `1 <= favoriteCompanies[i][j].length <= 20`
- `favoriteCompanies[i]` 中的所有字符串 __各不相同__ 。
- 用户收藏的公司清单也 __各不相同__ ，也就是说，即便我们按字母顺序排序每个清单，  `favoriteCompanies[i] != favoriteCompanies[j]` 仍然成立。
- 所有字符串仅包含小写英文字母。

__思路:__

```text
模拟
将所有字符串数组存入哈希表
比较哈希表的大小即可
时间复杂度为 O(MN ^ 2), 空间复杂度为 O(MN), M 为 favoriteCompanies 中每个字符串数组的大小, N 为 favoriteCompanies 数组的大小
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> peopleIndexes(vector<vector<string>>& favoriteCompanies) 
    {
        unordered_map<string, bitset<512>> m;
        vector<int> result;
        for (int i = 0, n = favoriteCompanies.size(); i < n; i++) for (const auto &word: favoriteCompanies[i]) m[word].set(i);
        for (int i = 0, n = favoriteCompanies.size(); i < n; i++) 
        {
            auto bits = m[favoriteCompanies[i].front()];
            for (int j = 1; j < favoriteCompanies[i].size(); j++) bits &= m[favoriteCompanies[i][j]];
            if (bits.count() <= 1) result.emplace_back(i);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> peopleIndexes(List<List<String>> favoriteCompanies) {
        List<Integer> result = new ArrayList<>();
        Set<Integer> set = new HashSet<>();
        Map<Integer, Set<String>> map = new HashMap<>();
        for (int i = 0, n = favoriteCompanies.size(); i < n; i++) map.put(i, new HashSet<>(favoriteCompanies.get(i)));
        for (int i = 0, n = favoriteCompanies.size(); i < n; i++) for (int j = 0; j < n; j++) if (i != j) if (map.get(j).containsAll(map.get(i))) set.add(i);
        for (int i = 0, n = favoriteCompanies.size(); i < n; i++) if (!set.contains(i)) result.add(i);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def peopleIndexes(self, favoriteCompanies: List[List[str]]) -> List[int]:
        return [i for i, f in enumerate(favoriteCompanies) if not any(set(c) > set(f) for c in favoriteCompanies)]
```
