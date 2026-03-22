# 853 Car Fleet 车队

__Description__:
There are n cars going to the same destination along a one-lane road. The destination is target miles away.

You are given two integer array position and speed, both of length n, where position[i] is the position of the ith car and speed[i] is the speed of the ith car (in miles per hour).

A car can never pass another car ahead of it, but it can catch up to it, and drive bumper to bumper at the same speed.

The distance between these two cars is ignored (i.e., they are assumed to have the same position).

A car fleet is some non-empty set of cars driving at the same position and same speed. Note that a single car is also a car fleet.

If a car catches up to a car fleet right at the destination point, it will still be considered as one car fleet.

Return the number of car fleets that will arrive at the destination.

__Example:__

Example 1:

Input: target = 12, position = [10,8,0,5,3], speed = [2,4,1,1,3]
Output: 3
Explanation:
The cars starting at 10 and 8 become a fleet, meeting each other at 12.
The car starting at 0 doesn't catch up to any other car, so it is a fleet by itself.
The cars starting at 5 and 3 become a fleet, meeting each other at 6.
Note that no other cars meet these fleets before the destination, so the answer is 3.

Example 2:

Input: target = 10, position = [3], speed = [3]
Output: 1

__Constraints:__

n == position.length == speed.length
1 <= n <= 10^5
0 < target <= 10^6
0 <= position[i] < target
All the values of position are unique.
0 < speed[i] <= 10^6

__题目描述__:
N  辆车沿着一条车道驶向位于 target 英里之外的共同目的地。

每辆车 i 以恒定的速度 speed[i] （英里/小时），从初始位置 position[i] （英里） 沿车道驶向目的地。

一辆车永远不会超过前面的另一辆车，但它可以追上去，并与前车以相同的速度紧接着行驶。

此时，我们会忽略这两辆车之间的距离，也就是说，它们被假定处于相同的位置。

车队 是一些由行驶在相同位置、具有相同速度的车组成的非空集合。注意，一辆车也可以是一个车队。

即便一辆车在目的地才赶上了一个车队，它们仍然会被视作是同一个车队。

会有多少车队到达目的地?

__示例 :__

输入：target = 12, position = [10,8,0,5,3], speed = [2,4,1,1,3]
输出：3
解释：
从 10 和 8 开始的车会组成一个车队，它们在 12 处相遇。
从 0 处开始的车无法追上其它车，所以它自己就是一个车队。
从 5 和 3 开始的车会组成一个车队，它们在 6 处相遇。
请注意，在到达目的地之前没有其它车会遇到这些车队，所以答案是 3。

__提示:__

0 <= N <= 10 ^ 4
0 < target <= 10 ^ 6
0 < speed[i] <= 10 ^ 6
0 <= position[i] < target
所有车的初始位置各不相同。

__思路__:

先按照 position 排序
然后计算各车到达 target 的时间
用单调栈或者从后往前遍历时间数组
时间较少的可以组合成一个车队
时间复杂度为 O(nlgn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int carFleet(int target, vector<int>& position, vector<int>& speed) 
    {
        map<int, int> cars;
        for (int i = 0; i < position.size(); i++) cars[position[i]] = speed[i];
        stack<float> s;
        for (auto& [position, v] : cars) 
        {
            float time = float(target - position) / v;
            while (!s.empty() and time >= s.top()) s.pop();
            s.push(time);
        }
        return s.size();
    }
};
```

__Java__:

```Java
class Solution 
{
    public int carFleet(int target, int[] position, int[] speed) 
    {
        TreeMap<Integer, Integer> cars = new TreeMap<>();
        for (int i = 0; i < position.length; i++) cars.put(position[i], speed[i]);
        Stack<Double> stack = new Stack<>();
        for (Map.Entry<Integer, Integer> entry : cars.entrySet()) {
            double time = ((double)(target - entry.getKey())) / entry.getValue();
            while (!stack.empty() && time >= stack.peek()) stack.pop();
            stack.push(time);
        }
        return stack.size();
    }
}
```

__Python__:

```Python
class Solution:
    def carFleet(self, target: int, position: List[int], speed: List[int]) -> int:
        cars = sorted(zip(position, speed))
        times, result = [float(target - p) / s for p, s in cars], 0
        while len(times) > 1:
            lead = times.pop()
            if lead < times[-1]: 
                result += 1
            else:
                times[-1] = lead
        return result + bool(times)
```
