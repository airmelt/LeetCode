# 1354 Construct Target Array With Multiple Sums 多次求和构造目标数组

__Description:__

You are given an array target of n integers. From a starting array arr consisting of n 1's, you may perform the following procedure :

let x be the sum of all elements currently in your array.
choose index i, such that 0 <= i < n and set the value of arr at index i to x.
You may repeat this procedure as many times as needed.
Return true if it is possible to construct the target array from arr, otherwise, return false.

__Example:__

Example 1:

Input: target = [9,3,5]
Output: true
Explanation: Start with arr = [1, 1, 1]
[1, 1, 1], sum = 3 choose index 1
[1, 3, 1], sum = 5 choose index 2
[1, 3, 5], sum = 9 choose index 0
[9, 3, 5] Done

Example 2:

Input: target = [1,1,1,2]
Output: false
Explanation: Impossible to create target array from [1,1,1,1].

Example 3:

Input: target = [8,5]
Output: true

__Constraints:__

n == target.length
1 <= n <= 5 * 10^4
1 <= target[i] <= 10^9

__题目描述：__

给你一个整数数组 target 。一开始，你有一个数组 A ，它的所有元素均为 1 ，你可以执行以下操作：

令 x 为你数组里所有元素的和
选择满足 0 <= i < target.size 的任意下标 i ，并让 A 数组里下标为 i 处的值为 x 。
你可以重复该过程任意次
如果能从 A 开始构造出目标数组 target ，请你返回 True ，否则返回 False 。

__示例：__

输入：target = [9,3,5]
输出：true
解释：从 [1, 1, 1] 开始
[1, 1, 1], 和为 3 ，选择下标 1
[1, 3, 1], 和为 5， 选择下标 2
[1, 3, 5], 和为 9， 选择下标 0
[9, 3, 5] 完成

示例 2：

输入：target = [1,1,1,2]
输出：false
解释：不可能从 [1,1,1,1] 出发构造目标数组。

示例 3：

输入：target = [8,5]
输出：true

__提示：__

N == target.length
1 <= target.length <= 5 * 10^4
1 <= target[i] <= 10^9

__思路：__

优先队列
逆推构造
每次取出最大值, 用数组其他数字和减去这个值, 这个值就可以替换
由于最大值可能操作多次, 比如 [1, 1000], 所以可以用最大值以及数组其他数字的和的取余来代替减法运算加快运算速度
时间复杂度为 O(nlgs), 空间复杂度为 O(n), 其中 s 表示数组的所有元素和

__代码__:

__C++__:

```C++
class Solution 
{
private:
    using LL = long long;
public:
    bool isPossible(vector<int>& target) 
    {
        priority_queue<LL> q(target.begin(), target.end());
        LL sum = accumulate(target.begin(), target.end(), 0L);
        while (q.top() != 1L)
        {
            LL cur = q.top(), gap = sum - cur, next = gap ? cur % gap : 1;
            q.pop();
            if (cur - gap < 1 or !gap) return false;
            if (!next) next = gap;
            sum -= cur - next;
            q.push(next);
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isPossible(int[] target) {
        PriorityQueue<Long> queue = new PriorityQueue<>();
        long sum = 0;
        for (int num : target) {
            queue.offer(-(long)num);
            sum += num;
        }
        while (queue.peek() != -1L) {
            long cur = -queue.poll(), gap = sum - cur, next = gap != 0 ? cur % gap : 1;
            if (cur - gap < 1 || gap == 0) return false;
            if (next == 0) next = gap;
            sum -= cur - next;
            queue.offer(-next);
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def isPossible(self, target: List[int]) -> bool:
        queue, s = [-t for t in target], sum(target)
        heapify(queue)
        while queue[0] != -1:
            top = cur % gap if (gap := s - (cur := -heappop(queue))) else 1
            if cur - gap < 1 or not gap:
                return False
            if not top:
                top = gap
            s -= cur - top
            heappush(queue, -top)
        return True
```
