# 2007 Find Original Array From Doubled Array 从双倍数组中还原原数组

__Description:__

An integer array `original` is transformed into a __doubled__ array `changed` by appending __twice the value__ of every element in `original`, and then randomly __shuffling__ the resulting array.

Given an array `changed`, return `original` _if_ `changed` _is a __doubled__ array. If_ `changed` _is not a __doubled__ array, return an empty array. The elements in_ `original` _may be returned in __any__ order_.

__Example:__

Example 1:

```text
Input: changed = [1,3,4,2,6,8]
Output: [1,3,4]
Explanation: One possible original array could be [1,3,4]:
```

- Twice the value of 1 is 1 * 2 = 2.
- Twice the value of 3 is 3 * 2 = 6.
- Twice the value of 4 is 4 * 2 = 8.
Other original arrays could be [4,3,1] or [3,1,4].

Example 2:

```text
Input: changed = [6,3,0,1]
Output: []
Explanation: changed is not a doubled array.
```

Example 3:

```text
Input: changed = [1]
Output: []
Explanation: changed is not a doubled array.
```

__Constraints:__

- `1 <= changed.length <= 10 ^ 5`
- `0 <= changed[i] <= 10 ^ 5`

__题目描述:__

一个整数数组 `original` 可以转变成一个 __双倍__ 数组 `changed` ，转变方式为将 `original` 中每个元素 __值乘以 2__ 加入数组中，然后将所有元素 __随机打乱__ 。

给你一个数组 `changed` ，如果 `change` 是 __双倍__ 数组，那么请你返回 `original`数组，否则请返回空数组。 `original` 的元素可以以 __任意__ 顺序返回。

__示例:__

示例 1：

```text
输入：changed = [1,3,4,2,6,8]
输出：[1,3,4]
解释：一个可能的 original 数组为 [1,3,4] :
```

- 将 1 乘以 2 ，得到 1 * 2 = 2 。
- 将 3 乘以 2 ，得到 3 * 2 = 6 。
- 将 4 乘以 2 ，得到 4 * 2 = 8 。
其他可能的原数组方案为 [4,3,1] 或者 [3,1,4] 。

示例 2：

```text
输入：changed = [6,3,0,1]
输出：[]
解释：changed 不是一个双倍数组。
```

示例 3：

```text
输入：changed = [1]
输出：[]
解释：changed 不是一个双倍数组。
```

__提示：__

- `1 <= changed.length <= 10 ^ 5`
- `0 <= changed[i] <= 10 ^ 5`

__思路:__

```text
排序 ➕ 队列
先判断 changed 数组的长度是否为奇数，若为奇数则返回空数组
将 changed 数组排序
从小到大遍历 changed 数组，如果当前元素的两倍值在队列中，则将队列中的元素弹出，并将当前元素的一半值加入结果数组中
否则将当前元素的两倍值加入队列中
最后判断队列是否为空，若为空则返回结果数组，否则返回空数组
时间复杂度为 O(NlgN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> findOriginalArray(vector<int>& changed) 
    {
        int n = changed.size();
        vector<int> result, empty;
        if ((n & 1) == 1) return empty;
        sort(changed.begin(), changed.end());
        queue<int> q;
        for (int i = 0; i < n; i++) 
        {
            if (!q.empty() and q.front() == changed[i]) 
            {
                result.emplace_back(q.front() >> 1);
                q.pop();
            }
            else q.push(changed[i] << 1);
        }
        return q.empty() ? result : empty;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] findOriginalArray(int[] changed) {
        int n = changed.length, result[] = new int[n >> 1], idx = 0, empty[] = new int[0];
        if ((n & 1) == 1) return empty;
        Arrays.sort(changed);
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            if (!queue.isEmpty() && queue.peek() == changed[i]) result[idx++] = queue.poll() >> 1;
            else queue.offer(changed[i] << 1);
        }
        return queue.isEmpty() ? result : empty;
    }
}
```

__Python__:

```Python
class Solution:
    def findOriginalArray(self, changed: List[int]) -> List[int]:
        result, n, queue, empty = [], len(changed), deque(), []
        if n & 1:
            return empty
        changed.sort()
        for i in range(n):
            if queue and queue[0] == changed[i]:
                result.append(queue.popleft() >> 1)
            else:
                queue.append(changed[i] << 1)
        return result if not queue else empty
```
