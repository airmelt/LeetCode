# 1488 Avoid Flood in The City 避免洪水泛滥

__Description:__

Your country has an infinite number of lakes. Initially, all the lakes are empty, but when it rains over the `nth` lake, the `nth` lake becomes full of water. If it rains over a lake that is __full of water__, there will be a __flood__. Your goal is to avoid floods in any lake.

Given an integer array `rains` where:

- `rains[i] > 0` means there will be rains over the `rains[i]` lake.
- `rains[i] == 0` means there are no rains this day and you can choose __one lake__ this day and __dry it__.

Return _an array `ans`_ where:

- `ans.length == rains.length`
- `ans[i] == -1` if `rains[i] > 0`.
- `ans[i]` is the lake you choose to dry in the `ith` day if `rains[i] == 0`.

If there are multiple valid answers return any of them. If it is impossible to avoid flood return an empty array.

Notice that if you chose to dry a full lake, it becomes empty, but if you chose to dry an empty lake, nothing changes.

__Example:__

Example 1:

```text
Input: rains = [1,2,3,4]
Output: [-1,-1,-1,-1]
Explanation: After the first day full lakes are [1]
After the second day full lakes are [1,2]
After the third day full lakes are [1,2,3]
After the fourth day full lakes are [1,2,3,4]
There's no day to dry any lake and there is no flood in any lake.
```

Example 2:

```text
Input: rains = [1,2,0,0,2,1]
Output: [-1,-1,2,1,-1,-1]
Explanation: After the first day full lakes are [1]
After the second day full lakes are [1,2]
After the third day, we dry lake 2. Full lakes are [1]
After the fourth day, we dry lake 1. There is no full lakes.
After the fifth day, full lakes are [2].
After the sixth day, full lakes are [1,2].
It is easy that this scenario is flood-free. [-1,-1,1,2,-1,-1] is another acceptable scenario.
```

Example 3:

```text
Input: rains = [1,2,0,1,2]
Output: []
Explanation: After the second day, full lakes are  [1,2]. We have to dry one lake in the third day.
After that, it will rain over lakes [1,2]. It's easy to prove that no matter which lake you choose to dry in the 3rd day, the other one will flood.
```

__Constraints:__

- `1 <= rains.length <= 10 ^ 5`
- `0 <= rains[i] <= 10 ^ 9`

__题目描述:__

你的国家有无数个湖泊，所有湖泊一开始都是空的。当第 `n` 个湖泊下雨前是空的，那么它就会装满水。如果第 `n` 个湖泊下雨前是 __满的__ ，这个湖泊会发生 __洪水__ 。你的目标是避免任意一个湖泊发生洪水。

给你一个整数数组 `rains` ，其中：

- `rains[i] > 0` 表示第 `i` 天时，第 `rains[i]` 个湖泊会下雨。
- `rains[i] == 0` 表示第 `i` 天没有湖泊会下雨，你可以选择 __一个__ 湖泊并 __抽干__ 这个湖泊的水。

请返回一个数组 __`ans` ，满足：

- `ans.length == rains.length`
- 如果 `rains[i] > 0` ，那么 `ans[i] == -1` 。
- 如果 `rains[i] == 0` ， `ans[i]` 是你第 `i` 天选择抽干的湖泊。

如果有多种可行解，请返回它们中的 任意一个 。如果没办法阻止洪水，请返回一个 空的数组 。

请注意，如果你选择抽干一个装满水的湖泊，它会变成一个空的湖泊。但如果你选择抽干一个空的湖泊，那么将无事发生。

__示例:__

示例 1：

```text
输入：rains = [1,2,3,4]
输出：[-1,-1,-1,-1]
解释：第一天后，装满水的湖泊包括 [1]
第二天后，装满水的湖泊包括 [1,2]
第三天后，装满水的湖泊包括 [1,2,3]
第四天后，装满水的湖泊包括 [1,2,3,4]
没有哪一天你可以抽干任何湖泊的水，也没有湖泊会发生洪水。
```

示例 2：

```text
输入：rains = [1,2,0,0,2,1]
输出：[-1,-1,2,1,-1,-1]
解释：第一天后，装满水的湖泊包括 [1]
第二天后，装满水的湖泊包括 [1,2]
第三天后，我们抽干湖泊 2 。所以剩下装满水的湖泊包括 [1]
第四天后，我们抽干湖泊 1 。所以暂时没有装满水的湖泊了。
第五天后，装满水的湖泊包括 [2]。
第六天后，装满水的湖泊包括 [1,2]。
可以看出，这个方案下不会有洪水发生。同时， [-1,-1,1,2,-1,-1] 也是另一个可行的没有洪水的方案。
```

示例 3：

```text
输入：rains = [1,2,0,1,2]
输出：[]
解释：第二天后，装满水的湖泊包括 [1,2]。我们可以在第三天抽干一个湖泊的水。
但第三天后，湖泊 1 和 2 都会再次下雨，所以不管我们第三天抽干哪个湖泊的水，另一个湖泊都会发生洪水。
```

__提示：__

- `1 <= rains.length <= 10 ^ 5`
- `0 <= rains[i] <= 10 ^ 9`

__思路:__

```text
贪心 ➕ 二分
初始化结果为 1, 保证每次都抽干至少一个湖泊
将每次涨水的时间和湖泊记录在一个哈希表中
将每次晴天的时间记录在一个有序列表中
如果天晴, rains[i] == 0, 此时将该天加入有序列表中
否则, 依据题意令 result[i] 为 -1
如果在下雨记录中找到了这个湖泊, 用二分查找查找晴天列表
如果有足够的晴天可以使用, 弹出该天并修改对应的 result 的值
否则直接返回空列表
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> avoidFlood(vector<int>& rains) 
    {
        vector<int> result(rains.size(), 1);
        unordered_map<int, int> record;
        set<int> left;
        for (int i = 0, n = rains.size(); i < n; i++)
        {
            if (!rains[i]) left.insert(i);
            else
            {
                result[i] = -1;
                if (record.count(rains[i]))
                {
                    auto pos = left.lower_bound(record[rains[i]]);
                    if (pos == left.end()) return vector<int>{};
                    result[*pos] = rains[i];
                    left.erase(pos);
                }
                record[rains[i]] = i;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] avoidFlood(int[] rains) {
        int n = rains.length, result[] = new int[n];
        Arrays.fill(result, 1);
        Map<Integer, Integer> record = new HashMap<>();
        TreeSet<Integer> left = new TreeSet<>();
        for (int i = 0; i < n; i++) {
            if (rains[i] == 0) left.add(i);
            else {
                result[i] = -1;
                if (record.containsKey(rains[i])) {
                    Integer pos = left.ceiling(record.get(rains[i]));
                    if (pos == null) return new int[0];
                    result[pos] = rains[i];
                    left.remove(pos);
                }
            }
            record.put(rains[i], i);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def avoidFlood(self, rains: List[int]) -> List[int]:
        result, left, record = [1] * len(rains), [], {}
        for i, val in enumerate(rains):
            if val:
                result[i] = -1
                if val in record:
                    if (pos := bisect_left(left, record[val])) >= len(left):
                        return []
                    result[left.pop(pos)] = val
                record[val] = i
            else:
                left.append(i)
        return result
```
