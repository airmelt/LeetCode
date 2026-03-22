# 2115 Find All Possible Recipes from Given Supplies 从给定原材料中找到所有可以做出的菜

__Description:__

You have information about `n` different recipes. You are given a string array `recipes` and a 2D string array `ingredients`. The `i ^ th` recipe has the name `recipes[i]`, and you can __create__ it if you have __all__ the needed ingredients from `ingredients[i]`. Ingredients to a recipe may need to be created from __other__ recipes, i.e., `ingredients[i]` may contain a string that is in `recipes`.

You are also given a string array `supplies` containing all the ingredients that you initially have, and you have an infinite supply of all of them.

Return a list of all the recipes that you can create. You may return the answer in any order.

Note that two recipes may contain each other in their ingredients.

__Example:__

Example 1:

```text
Input: recipes = ["bread"], ingredients = [["yeast","flour"]], supplies = ["yeast","flour","corn"]
Output: ["bread"]
Explanation:
We can create "bread" since we have the ingredients "yeast" and "flour".
```

Example 2:

```text
Input: recipes = ["bread","sandwich"], ingredients = [["yeast","flour"],["bread","meat"]], supplies = ["yeast","flour","meat"]
Output: ["bread","sandwich"]
Explanation:
We can create "bread" since we have the ingredients "yeast" and "flour".
We can create "sandwich" since we have the ingredient "meat" and can create the ingredient "bread".
```

Example 3:

```text
Input: recipes = ["bread","sandwich","burger"], ingredients = [["yeast","flour"],["bread","meat"],["sandwich","meat","bread"]], supplies = ["yeast","flour","meat"]
Output: ["bread","sandwich","burger"]
Explanation:
We can create "bread" since we have the ingredients "yeast" and "flour".
We can create "sandwich" since we have the ingredient "meat" and can create the ingredient "bread".
We can create "burger" since we have the ingredient "meat" and can create the ingredients "bread" and "sandwich".
```

__Constraints:__

- `n == recipes.length == ingredients.length`
- `1 <= n <= 100`
- `1 <= ingredients[i].length, supplies.length <= 100`
- `1 <= recipes[i].length, ingredients[i][j].length, supplies[k].length <= 10`
- `recipes[i], ingredients[i][j]`, and `supplies[k]` consist only of lowercase English letters.
- All the values of `recipes` and `supplies` combined are unique.
- Each `ingredients[i]` does not contain any duplicate values.

__题目描述:__

你有 `n` 道不同菜的信息。给你一个字符串数组 `recipes` 和一个二维字符串数组 `ingredients` 。第 `i` 道菜的名字为 `recipes[i]` ，如果你有它 __所有__ 的原材料 `ingredients[i]` ，那么你可以 __做出__ 这道菜。一道菜的原材料可能是 __另一道__ 菜，也就是说 `ingredients[i]` 可能包含 `recipes` 中另一个字符串。

同时给你一个字符串数组 `supplies` ，它包含你初始时拥有的所有原材料，每一种原材料你都有无限多。

请你返回你可以做出的所有菜。你可以以 任意顺序 返回它们。

注意两道菜在它们的原材料中可能互相包含。

__示例:__

示例 1：

```text
输入：recipes = ["bread"], ingredients = [["yeast","flour"]], supplies = ["yeast","flour","corn"]
输出：["bread"]
解释：
我们可以做出 "bread" ，因为我们有原材料 "yeast" 和 "flour" 。
```

示例 2：

```text
输入：recipes = ["bread","sandwich"], ingredients = [["yeast","flour"],["bread","meat"]], supplies = ["yeast","flour","meat"]
输出：["bread","sandwich"]
解释：
我们可以做出 "bread" ，因为我们有原材料 "yeast" 和 "flour" 。
我们可以做出 "sandwich" ，因为我们有原材料 "meat" 且可以做出原材料 "bread" 。
```

示例 3：

```text
输入：recipes = ["bread","sandwich","burger"], ingredients = [["yeast","flour"],["bread","meat"],["sandwich","meat","bread"]], supplies = ["yeast","flour","meat"]
输出：["bread","sandwich","burger"]
解释：
我们可以做出 "bread" ，因为我们有原材料 "yeast" 和 "flour" 。
我们可以做出 "sandwich" ，因为我们有原材料 "meat" 且可以做出原材料 "bread" 。
我们可以做出 "burger" ，因为我们有原材料 "meat" 且可以做出原材料 "bread" 和 "sandwich" 。
```

示例 4：

```text
输入：recipes = ["bread"], ingredients = [["yeast","flour"]], supplies = ["yeast"]
输出：[]
解释：
我们没法做出任何菜，因为我们只有原材料 "yeast" 。
```

__提示：__

- `n == recipes.length == ingredients.length`
- `1 <= n <= 100`
- `1 <= ingredients[i].length, supplies.length <= 100`
- `1 <= recipes[i].length, ingredients[i][j].length, supplies[k].length <= 10`
- `recipes[i], ingredients[i][j]` 和 `supplies[k]` 只包含小写英文字母。
- 所有 `recipes` 和 `supplies` 中的值互不相同。
- `ingredients[i]` 中的字符串互不相同。

__思路:__

```text
拓扑排序
将每道菜需要的原料加入哈希字典
将原料加入一个队列
遍历队列
移除原材料
如果菜对应的值为空说明可以制作加入到结果中
并将这道菜也作为原材料加入队列
时间复杂度为 O(N + M), 空间复杂度为 O(N + M), 其中 N 为菜的个数, M 为原材料的个数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<string> findAllRecipes(vector<string>& recipes, vector<vector<string>>& ingredients, vector<string>& supplies) 
    {
        int n = recipes.size();
        unordered_map<string, unordered_set<string>> graph;
        for (int i = 0; i < n; i++) for (const auto& ingredient : ingredients[i]) graph[recipes[i]].insert(ingredient);
        vector<string> result;
        queue<string> q;
        for (const auto& supply : supplies) q.push(supply);
        while (!q.empty()) 
        {
            auto cur = q.front();
            q.pop();
            for (const auto& [ingredient, _]: graph) 
            {
                if (!graph[ingredient].empty())
                {
                    if (graph[ingredient].count(cur)) 
                    {
                        graph[ingredient].erase(cur);
                        if (graph[ingredient].empty()) 
                        {
                            result.emplace_back(ingredient);
                            q.push(ingredient);
                        }
                    }
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> findAllRecipes(String[] recipes, List<List<String>> ingredients, String[] supplies) {
        List<String> result = new ArrayList<>();
        Map<String, Set<String>> graph = new HashMap<>();
        int n = recipes.length;
        for (int i = 0; i < n; i++) {
            Set<String> set = new HashSet<>();
            for (String ingredient : ingredients.get(i)) set.add(ingredient);
            graph.put(recipes[i], set);
        }
        Queue<String> q = new LinkedList<>();
        for (String supply : supplies) q.offer(supply);
        while (!q.isEmpty()) {
            String cur = q.poll();
            for (String ingredient : graph.keySet()) {
                if (!graph.get(ingredient).isEmpty()) {
                    if (graph.get(ingredient).contains(cur)) {
                        graph.get(ingredient).remove(cur);
                        if (graph.get(ingredient).isEmpty()) {
                            result.add(ingredient);
                            q.add(ingredient);
                        }
                    }
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findAllRecipes(self, recipes: List[str], ingredients: List[List[str]], supplies: List[str]) -> List[str]:
        graph, n, indegree, result, q = defaultdict(set), len(recipes), Counter(), [], deque(supplies)
        for i in range(n):
            for ingredient in ingredients[i]:
                graph[ingredient].add(recipes[i])
            indegree[recipes[i]] = len(ingredients[i])
        while q:
            cur = q.popleft()
            for ingredient in graph[cur]:
                indegree[ingredient] -= 1
                if not indegree[ingredient]:
                    result.append(ingredient)
                    q.append(ingredient)
        return result
```
