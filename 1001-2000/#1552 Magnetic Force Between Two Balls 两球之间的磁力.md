# 1552 Magnetic Force Between Two Balls 两球之间的磁力

__Description:__

In the universe Earth C-137, Rick discovered a special form of magnetic force between two balls if they are put in his new invented basket. Rick has `n` empty baskets, the `i ^ th` basket is at `position[i]`, Morty has `m` balls and needs to distribute the balls into the baskets such that the __minimum magnetic force__ between any two balls is __maximum__.

Rick stated that magnetic force between two different balls at positions `x` and `y` is `|x - y|`.

Given the integer array `position` and the integer `m`. Return _the required force_.

__Example:__

Example 1:

![1552-1](https://assets.leetcode.com/uploads/2020/08/11/q3v1.jpg)

```text
Input: position = [1,2,3,4,7], m = 3
Output: 3
Explanation: Distributing the 3 balls into baskets 1, 4 and 7 will make the magnetic force between ball pairs [3, 3, 6]. The minimum magnetic force is 3. We cannot achieve a larger minimum magnetic force than 3.
```

Example 2:

```text
Input: position = [5,4,3,2,1,1000000000], m = 2
Output: 999999999
Explanation: We can use baskets 1 and 1000000000.
```

__Constraints:__

- `n == position.length`
- `2 <= n <= 10 ^ 5`
- `1 <= position[i] <= 10 ^ 9`
- All integers in `position` are __distinct__.
- `2 <= m <= position.length`

__题目描述:__

在代号为 C-137 的地球上，Rick 发现如果他将两个球放在他新发明的篮子里，它们之间会形成特殊形式的磁力。Rick 有 `n` 个空的篮子，第 `i` 个篮子的位置在 `position[i]` ，Morty 想把 `m` 个球放到这些篮子里，使得任意两球间 __最小磁力__ 最大。

已知两个球如果分别位于 `x` 和 `y` ，那么它们之间的磁力为 `|x - y|` 。

给你一个整数数组 `position` 和一个整数 `m` ，请你返回最大化的最小磁力。

__示例:__

示例 1：

![1552-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/16/q3v1.jpg)

```text
输入：position = [1,2,3,4,7], m = 3
输出：3
解释：将 3 个球分别放入位于 1，4 和 7 的三个篮子，两球间的磁力分别为 [3, 3, 6]。最小磁力为 3 。我们没办法让最小磁力大于 3 。
```

示例 2：

```text
输入：position = [5,4,3,2,1,1000000000], m = 2
输出：999999999
解释：我们使用位于 1 和 1000000000 的篮子时最小磁力最大。
```

__提示：__

- `n == position.length`
- `2 <= n <= 10 ^ 5`
- `1 <= position[i] <= 10 ^ 9`
- 所有 `position` 中的整数 __互不相同__ 。
- `2 <= m <= position.length`

__思路:__

```text
二分法
从最大化最小值可以考虑使用二分法
用二分法查找这个最小值
反向思考, 需要检查这个最小值最多可以放置多少个球
先将数组进行排序
初始化 left = 1, right = (position[-1] - position[0]) // (m - 1), 实际上 left 可以取到 min(position[i + 1] - position[i]), right 的取值为均匀分布 m 个球的最大间隔
令每次查找 mid = (left + right + 1) >> 1, 如果有 m 个球的间隔不小于 mid, 则查找区间小了, left = mid, 否则查找区间大了, right = mid - 1
时间复杂度为 O(NlogNS), 空间复杂度为 O(logN), 其中 S = MAX(position) - MIN(position)
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    bool check(vector<int>& position, int m, int mid) 
    {
        int pre = position[0], count = 1;
        for (const auto& pos: position) 
        {
            if (pos - pre >= mid) 
            {
                pre = pos;
                if (++count >= m) return true;
            }
        }
        return false;
    }
public:
    int maxDistance(vector<int>& position, int m) 
    {
        sort(position.begin(), position.end());
        int left = 1, right = (position.back() - position.front()) / (m - 1);
        while (left < right) 
        {
            int mid = left + ((right - left + 1) >> 1);
            if (check(position, m, mid)) left = mid;
            else right = mid - 1;
        }
        return left;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxDistance(int[] position, int m) {
        Arrays.sort(position);
        int n = position.length, left = 1, right = (position[n - 1] - position[0]) / (m - 1);
        while (left < right) {
            int mid = left + ((right - left + 1) >>> 1);
            if (check(position, m, mid)) left = mid;
            else right = mid - 1;
        }
        return left;
    }
    
    private boolean check(int[] position, int m, int mid) {
        int pre = position[0], count = 1;
        for (int pos: position) {
            if (pos - pre >= mid) {
                pre = pos;
                if (++count >= m) return true;
            }
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def maxDistance(self, position: List[int], m: int) -> int:
        position.sort()
        
        def check(mid: int) -> bool:
            count, pre = 1, position[0]
            for pos in position[1:]:
                if pos - pre >= mid:
                    pre = pos
                    count += 1
            return count >= m
        left, right = 1, (position[-1] - position[0]) // (m - 1)
        while left < right:
            if (check(mid := left + right + 1 >> 1)):
                left = mid
            else:
                right = mid - 1
        return left
```
