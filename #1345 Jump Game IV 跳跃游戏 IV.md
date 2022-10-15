# 1345 Jump Game IV 跳跃游戏 IV

__Description:__

Given an array of integers arr, you are initially positioned at the first index of the array.

In one step you can jump from index i to index:

i + 1 where: i + 1 < arr.length.
i - 1 where: i - 1 >= 0.
j where: arr[i] == arr[j] and i != j.
Return the minimum number of steps to reach the last index of the array.

Notice that you can not jump outside of the array at any time.

__Example:__

Example 1:

Input: arr = [100,-23,-23,404,100,23,23,23,3,404]
Output: 3
Explanation: You need three jumps from index 0 --> 4 --> 3 --> 9. Note that index 9 is the last index of the array.

Example 2:

Input: arr = [7]
Output: 0
Explanation: Start index is the last index. You do not need to jump.

Example 3:

Input: arr = [7,6,9,6,9,6,9,7]
Output: 1
Explanation: You can jump directly from index 0 to index 7 which is last index of the array.

__Constraints:__

1 <= arr.length <= 5 * 10^4
-10^8 <= arr[i] <= 10^8

__题目描述：__

给你一个整数数组 arr ，你一开始在数组的第一个元素处（下标为 0）。

每一步，你可以从下标 i 跳到下标 i + 1 、i - 1 或者 j ：

i + 1 需满足：i + 1 < arr.length
i - 1 需满足：i - 1 >= 0
j 需满足：arr[i] == arr[j] 且 i != j
请你返回到达数组最后一个元素的下标处所需的 最少操作次数 。

注意：任何时候你都不能跳到数组外面。

__Constraints:__

示例 1：

输入：arr = [100,-23,-23,404,100,23,23,23,3,404]
输出：3
解释：那你需要跳跃 3 次，下标依次为 0 --> 4 --> 3 --> 9 。下标 9 为数组的最后一个元素的下标。

示例 2：

输入：arr = [7]
输出：0
解释：一开始就在最后一个元素处，所以你不需要跳跃。

示例 3：

输入：arr = [7,6,9,6,9,6,9,7]
输出：1
解释：你可以直接从下标 0 处跳到下标 7 处，也就是数组的最后一个元素处。

__提示：__

1 <= arr.length <= 5 * 10^4
-10^8 <= arr[i] <= 10^8

__思路：__

BFS
将相同值对应的下标加入哈希表, 连续多个相同值可以跳过, 因为一定用不上
将当前下标以及步数加入队列中, 遍历哈希表中的值, 这些值都可以一次到达
然后加入前一个下标和后一个下标
并用 visited 数组记录访问过的下标
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution {
public:
    int minJumps(vector<int>& arr) {
        queue<pair<int, int>> q;
        q.push(make_pair(0, 0));
        unordered_set<int> visited;
        visited.insert(0);
        unordered_map<int, vector<int>> m;
        int n = arr.size();
        for (int i = 0, last = -1; i < n; i++) {
            while (i > 0 and i < n - 1 and last == arr[i] and last == arr[i + 1]) i++;
            m[arr[i]].emplace_back(i);
            last = arr[i];
        }
        while(!q.empty()) 
        {
            int cur = q.front().first, step = q.front().second;
            q.pop();
            if (cur == n - 1) return step;
            ++step;
            if (m.count(arr[cur])) 
            {
                for (const auto& neighbor : m[arr[cur]]) 
                {
                    if (!visited.count(neighbor)) 
                    {
                        q.push(make_pair(neighbor, step));
                        visited.insert(neighbor);
                    }
                }
                m.erase(arr[cur]);
            }
            if (cur > 0 and !visited.count(cur - 1)) 
            {
                q.push(make_pair(cur - 1, step));
                visited.insert(cur - 1);
            }
            if (cur < n - 1 and !visited.count(cur + 1)) 
            {
                q.push(make_pair(cur + 1, step));
                visited.insert(cur + 1);
            }
        }
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minJumps(int[] arr) {
        Queue<int[]> queue = new LinkedList<>(){{ offer(new int[]{0, 0}); }};
        Set<Integer> visited = new HashSet<>(){{ add(0); }};
        Map<Integer, List<Integer>> map = new HashMap<>();
        int n = arr.length;
        for (int i = 0, last = -1; i < n; i++) {
            while (i > 0 && i < n - 1 && last == arr[i] && last == arr[i + 1]) i++;
            map.computeIfAbsent(arr[i], x -> new ArrayList<>()).add(i);
            last = arr[i];
        }
        while(!queue.isEmpty()) {
            int cur = queue.peek()[0], step = queue.poll()[1];
            if (cur == n - 1) return step;
            ++step;
            if (map.containsKey(arr[cur])) {
                for (int neighbor : map.get(arr[cur])) {
                    if (!visited.contains(neighbor)) {
                        queue.offer(new int[] {neighbor, step});
                        visited.add(neighbor);
                    }
                }
                map.remove(arr[cur]);
            }
            if (cur > 0 && !visited.contains(cur - 1)) {
                queue.offer(new int[]{cur - 1, step});
                visited.add(cur - 1);
            }
            if (cur < n - 1 && !visited.contains(cur + 1)) {
                queue.offer(new int[]{cur + 1, step});
                visited.add(cur + 1);
            }
        }
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def minJumps(self, arr: List[int]) -> int:
        next_jump, visited, q, n = defaultdict(list), {0}, deque([[0, 0]]), len(arr)
        for i, a in enumerate(arr):
            next_jump[a].append(i)
        while q:
            i, step = q.popleft()
            if i == n - 1:
                return step
            v = arr[i]
            step += 1
            for next_i in next_jump[v]:
                if next_i not in visited:
                    visited.add(next_i)
                    q.append([next_i, step])
            del next_jump[v]
            if i + 1 < n and (i + 1) not in visited:
                visited.add(i + 1)
                q.append([i+1, step])
            if i - 1 > -1 and (i - 1) not in visited:
                visited.add(i - 1)
                q.append([i - 1, step])
```
