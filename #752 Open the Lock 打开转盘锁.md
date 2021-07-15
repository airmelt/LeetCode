# 752 Open the Lock 打开转盘锁

__Description__:
You have a lock in front of you with 4 circular wheels. Each wheel has 10 slots: '0', '1', '2', '3', '4', '5', '6', '7', '8', '9'. The wheels can rotate freely and wrap around: for example we can turn '9' to be '0', or '0' to be '9'. Each move consists of turning one wheel one slot.

The lock initially starts at '0000', a string representing the state of the 4 wheels.

You are given a list of deadends dead ends, meaning if the lock displays any of these codes, the wheels of the lock will stop turning and you will be unable to open it.

Given a target representing the value of the wheels that will unlock the lock, return the minimum total number of turns required to open the lock, or -1 if it is impossible.

__Example:__

Example 1:

Input: deadends = ["0201","0101","0102","1212","2002"], target = "0202"
Output: 6
Explanation:
A sequence of valid moves would be "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202".
Note that a sequence like "0000" -> "0001" -> "0002" -> "0102" -> "0202" would be invalid,
because the wheels of the lock become stuck after the display becomes the dead end "0102".

Example 2:

Input: deadends = ["8888"], target = "0009"
Output: 1
Explanation:
We can turn the last wheel in reverse to move from "0000" -> "0009".

Example 3:

Input: deadends = ["8887","8889","8878","8898","8788","8988","7888","9888"], target = "8888"
Output: -1
Explanation:
We can't reach the target without getting stuck.

Example 4:

Input: deadends = ["0000"], target = "8888"
Output: -1

__Constraints:__

1 <= deadends.length <= 500
deadends[i].length == 4
target.length == 4
target will not be in the list deadends.
target and deadends[i] consist of digits only.

__题目描述__:
你有一个带有四个圆形拨轮的转盘锁。每个拨轮都有10个数字： '0', '1', '2', '3', '4', '5', '6', '7', '8', '9' 。每个拨轮可以自由旋转：例如把 '9' 变为 '0'，'0' 变为 '9' 。每次旋转都只能旋转一个拨轮的一位数字。

锁的初始数字为 '0000' ，一个代表四个拨轮的数字的字符串。

列表 deadends 包含了一组死亡数字，一旦拨轮的数字和列表里的任何一个元素相同，这个锁将会被永久锁定，无法再被旋转。

字符串 target 代表可以解锁的数字，你需要给出解锁需要的最小旋转次数，如果无论如何不能解锁，返回 -1 。

__示例 :__

示例 1:

输入：deadends = ["0201","0101","0102","1212","2002"], target = "0202"
输出：6
解释：
可能的移动序列为 "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202"。
注意 "0000" -> "0001" -> "0002" -> "0102" -> "0202" 这样的序列是不能解锁的，
因为当拨动到 "0102" 时这个锁就会被锁定。

示例 2:

输入: deadends = ["8888"], target = "0009"
输出：1
解释：
把最后一位反向旋转一次即可 "0000" -> "0009"。

示例 3:

输入: deadends = ["8887","8889","8878","8898","8788","8988","7888","9888"], target = "8888"
输出：-1
解释：
无法旋转到目标数字且不被锁定。

示例 4:

输入: deadends = ["0000"], target = "8888"
输出：-1

__提示:__

1 <= deadends.length <= 500
deadends[i].length == 4
target.length == 4
target 不在 deadends 之中
target 和 deadends[i] 仅由若干位数字组成

__思路__:

BFS
将 deadends 中的字符串存入 set 中加快查找
dead 中不能有 "0000", 否则直接返回 -1
然后用队列存放每次查找到的字符串
每次更新可以对 4 个位置上的数分别进行 +1 和 -1 的操作
时间复杂度为 O(m), 空间复杂度为 O(m), 因为只有 4 位可以操作, 时间复杂度和空间复杂度仅由 deadends 的长度决定

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int openLock(vector<string>& deadends, string target) 
    {
        unordered_set<string> dead(deadends.begin(), deadends.end());
        if (dead.count("0000")) return -1; 
        dead.insert("0000");
        queue<string> q; 
        q.push("0000"); 
        int step = 0;
        string cur, next;
        while (!q.empty()) 
        {
            for (int i = q.size(); i > 0; i--) 
            {
                cur = q.front(); 
                q.pop();
                if (cur == target) return step;
                for (int j = 0; j < 4; j++) 
                {
                    for (int k = -1; k <= 1; k += 2) 
                    {
                        next = cur;
                        if (cur[j] == '0' and k == -1) next[j] = '9';
                        else if (cur[j] == '9' and k == 1) next[j] = '0';
                        else next[j] = cur[j] + k;
                        if (next == target) return step + 1;
                        if (!dead.count(next)) 
                        {
                            dead.insert(next); 
                            q.push(next);
                        } 
                    }
                } 
            }
            ++step;
        }
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int openLock(String[] deadends, String target) {
        Set<String> dead = new HashSet<>(Arrays.asList(deadends));
        if (dead.contains("0000")) return -1; 
        dead.add("0000");
        Queue<String> queue = new LinkedList<>();
        queue.offer("0000"); 
        int step = 0;
        String cur = new String();
        StringBuilder next = new StringBuilder();
        while (!queue.isEmpty()) {
            for (int i = queue.size(); i > 0; i--) {
                cur = queue.poll();
                if (cur.equals(target)) return step;
                for (int j = 0; j < 4; j++) {
                    for (int k = -1; k <= 1; k += 2) {
                        next = new StringBuilder(cur);
                        if (cur.charAt(j) == '0' && k == -1) next.setCharAt(j, '9');
                        else if (cur.charAt(j) == '9' && k == 1) next.setCharAt(j, '0');
                        else next.setCharAt(j, (char)(cur.charAt(j) + k));
                        if (next.toString().equals(target)) return step + 1;
                        if (!dead.contains(next.toString())) {
                            dead.add(next.toString());
                            queue.offer(next.toString());
                        } 
                    }
                } 
            }
            ++step;
        }
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def openLock(self, deadends: List[str], target: str) -> int:
        dead, visited, step = set(deadends), set(), 0
        if "0000" in dead:
            return -1
        q = collections.deque()
        q.append("0000")
        visited.add("0000")
        
        def plus_one(x: str, i: int) -> str:
            x = list(x)
            t = (int(x[i]) + 1) % 10
            x[i] = str(t)
            return ''.join(x)
    
        def sub_one(x: str, i: int) -> str:
            x = list(x)
            t = (int(x[i]) - 1 + 10) % 10
            x[i] = str(t)
            return ''.join(x)
        
        while q:
            cur_len = len(q)
            for _ in range(cur_len):
                x = q.popleft()
                if x == target:
                    return step
                if x in dead:
                    continue
                for i in range(4):
                    y = plus_one(x, i)
                    if y not in visited:
                        q.append(y)
                        visited.add(y)
                    y = sub_one(x, i)
                    if y not in visited:
                        q.append(y)
                        visited.add(y)
            step += 1
        return -1
```
