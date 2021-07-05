# 735 Asteroid Collision 行星碰撞

__Description__:
We are given an array asteroids of integers representing asteroids in a row.

For each asteroid, the absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.

__Example:__

Example 1:

Input: asteroids = [5,10,-5]
Output: [5,10]
Explanation: The 10 and -5 collide resulting in 10. The 5 and 10 never collide.

Example 2:

Input: asteroids = [8,-8]
Output: []
Explanation: The 8 and -8 collide exploding each other.

Example 3:

Input: asteroids = [10,2,-5]
Output: [10]
Explanation: The 2 and -5 collide resulting in -5. The 10 and -5 collide resulting in 10.

Example 4:

Input: asteroids = [-2,-1,1,2]
Output: [-2,-1,1,2]
Explanation: The -2 and -1 are moving left, while the 1 and 2 are moving right. Asteroids moving the same direction never meet, so no asteroids will meet each other.

__Constraints:__

2 <= asteroids.length <= 10^4
-1000 <= asteroids[i] <= 1000
asteroids[i] != 0

__题目描述__:
给定一个整数数组 asteroids，表示在同一行的行星。

对于数组中的每一个元素，其绝对值表示行星的大小，正负表示行星的移动方向（正表示向右移动，负表示向左移动）。每一颗行星以相同的速度移动。

找出碰撞后剩下的所有行星。碰撞规则：两个行星相互碰撞，较小的行星会爆炸。如果两颗行星大小相同，则两颗行星都会爆炸。两颗移动方向相同的行星，永远不会发生碰撞。

__示例 :__

示例 1：

输入：asteroids = [5,10,-5]
输出：[5,10]
解释：10 和 -5 碰撞后只剩下 10 。 5 和 10 永远不会发生碰撞。

示例 2：

输入：asteroids = [8,-8]
输出：[]
解释：8 和 -8 碰撞后，两者都发生爆炸。

示例 3：

输入：asteroids = [10,2,-5]
输出：[10]
解释：2 和 -5 发生碰撞后剩下 -5 。10 和 -5 发生碰撞后剩下 10 。

示例 4：

输入：asteroids = [-2,-1,1,2]
输出：[-2,-1,1,2]
解释：-2 和 -1 向左移动，而 1 和 2 向右移动。 由于移动方向相同的行星不会发生碰撞，所以最终没有行星发生碰撞。

__提示:__

2 <= asteroids.length <= 10^4
-1000 <= asteroids[i] <= 1000
asteroids[i] != 0

__思路__:

栈
首先注意到如果负数在数组的左边或者正数在数组的右边是不会发生碰撞的
所以只有当数组中出现正数时才考虑存入栈中
当遇到负数时, 如果栈为空, 说明不会相撞, 直接将这个负数放入结果中
如果栈不为空, 比较栈顶和负数的大小关系
如果栈顶比负数的绝对值小, 弹出栈顶, 直到栈为空或者栈顶不小于负数的绝对值
然后判断栈顶和负数的绝对值是否相等, 相等的话两个行星一起湮灭
最后将所有栈中元素加入结果中即可
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> asteroidCollision(vector<int>& asteroids) 
    {
        vector<int> result;
        for(int i = 0; i < asteroids.size(); i++) 
        {
            while(!result.empty() and result.back() > 0 and asteroids[i] < 0) 
            {
                int cur = result.back();
                result.pop_back();
                if (cur > -asteroids[i]) asteroids[i] = cur;
                else if (cur == -asteroids[i]) asteroids[i] = 0;
            }
            if (asteroids[i]) result.emplace_back(asteroids[i]);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] asteroidCollision(int[] asteroids) {
        List<Integer> list = new ArrayList<>();
        Stack<Integer> stack = new Stack<>();
        for (int a : asteroids) {
            if (a > 0) stack.push(a);
            else {
                while (!stack.isEmpty() && stack.peek() < -a) stack.pop();
                if (!stack.isEmpty() && stack.peek() == -a) stack.pop();
                else if (stack.isEmpty()) list.add(a);
            }
        }
        int[] result = new int[list.size() + stack.size()];
        for (int i = 0, n = list.size(); i < n; i++) result[i] = list.get(i);
        for (int i = result.length - 1, m = list.size(); i >= m; i--) result[i] = stack.pop();
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def asteroidCollision(self, asteroids: List[int]) -> List[int]:
        result, stack = [], []
        for a in asteroids:
            if a > 0:
                stack.append(a)
            else:
                while stack and stack[-1] < -a:
                    stack.pop()
                if stack and stack[-1] == -a:
                    stack.pop()
                elif not stack:
                    result.append(a)
        result += stack
        return result
```
