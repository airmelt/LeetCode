__Description__:
We distribute some number of candies, to a row of n = num_people people in the following way:

We then give 1 candy to the first person, 2 candies to the second person, and so on until we give n candies to the last person.

Then, we go back to the start of the row, giving n + 1 candies to the first person, n + 2 candies to the second person, and so on until we give 2 * n candies to the last person.

This process repeats (with us giving one more candy each time, and moving to the start of the row after we reach the end) until we run out of candies.  The last person will receive all of our remaining candies (not necessarily one more than the previous gift).

Return an array (of length num_people and sum candies) that represents the final distribution of candies.

__Example:__
Example 1:

Input: candies = 7, num_people = 4
Output: [1,2,3,1]
Explanation:
On the first turn, ans[0] += 1, and the array is [1,0,0,0].
On the second turn, ans[1] += 2, and the array is [1,2,0,0].
On the third turn, ans[2] += 3, and the array is [1,2,3,0].
On the fourth turn, ans[3] += 1 (because there is only one candy left), and the final array is [1,2,3,1].

Example 2:

Input: candies = 10, num_people = 3
Output: [5,2,3]
Explanation: 
On the first turn, ans[0] += 1, and the array is [1,0,0].
On the second turn, ans[1] += 2, and the array is [1,2,0].
On the third turn, ans[2] += 3, and the array is [1,2,3].
On the fourth turn, ans[0] += 4, and the final array is [5,2,3].
 
__Constraints:__

1 <= candies <= 10^9
1 <= num_people <= 1000

__题目描述__:
排排坐，分糖果。

我们买了一些糖果 candies，打算把它们分给排好队的 n = num_people 个小朋友。

给第一个小朋友 1 颗糖果，第二个小朋友 2 颗，依此类推，直到给最后一个小朋友 n 颗糖果。

然后，我们再回到队伍的起点，给第一个小朋友 n + 1 颗糖果，第二个小朋友 n + 2 颗，依此类推，直到给最后一个小朋友 2 * n 颗糖果。

重复上述过程（每次都比上一次多给出一颗糖果，当到达队伍终点后再次从队伍起点开始），直到我们分完所有的糖果。注意，就算我们手中的剩下糖果数不够（不比前一次发出的糖果多），这些糖果也会全部发给当前的小朋友。

返回一个长度为 num_people、元素之和为 candies 的数组，以表示糖果的最终分发情况（即 ans[i] 表示第 i 个小朋友分到的糖果数）。

__示例 :__
示例 1：

输入：candies = 7, num_people = 4
输出：[1,2,3,1]
解释：
第一次，ans[0] += 1，数组变为 [1,0,0,0]。
第二次，ans[1] += 2，数组变为 [1,2,0,0]。
第三次，ans[2] += 3，数组变为 [1,2,3,0]。
第四次，ans[3] += 1（因为此时只剩下 1 颗糖果），最终数组变为 [1,2,3,1]。

示例 2：

输入：candies = 10, num_people = 3
输出：[5,2,3]
解释：
第一次，ans[0] += 1，数组变为 [1,0,0]。
第二次，ans[1] += 2，数组变为 [1,2,0]。
第三次，ans[2] += 3，数组变为 [1,2,3]。
第四次，ans[0] += 4，最终数组变为 [5,2,3]。
 
__提示：__

1 <= candies <= 10^9
1 <= num_people <= 1000

__思路__:
1. 按照题目要求分配
时间复杂度O(n), 空间复杂度O(1)
2. 数学法: 除去最后一次分配, 前 n次分配和应该是 n * (n + 1) / 2个糖果, 可以解一元二次方程得到 n
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<int> distributeCandies(int candies, int num_people) 
    {
        vector<int> result(num_people, 0);
        int count = 0;
        while (candies > 0) 
        {
            result[(count - 1) % num_people] += min(++count, candies);
            candies -= count;
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int[] distributeCandies(int candies, int num_people) {
        int result[] = new int[num_people], count = 0;
        while (candies > 0) {
            result[count % num_people] += Math.min(++count, candies);
            candies -= count;
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def distributeCandies(self, candies: int, num_people: int) -> List[int]:
        return [(((2 * x + (int((-1 + (1 + 8 * candies) ** 0.5) / 2) - x) // num_people * num_people) * (1 + (int((-1 + (1 + 8 * candies) ** 0.5) / 2) - x) // num_people)) // 2 + (x == int((-1 + (1 + 8 * candies) ** 0.5) / 2) % num_people + 1) * (candies - (int((-1 + (1 + 8 * candies) ** 0.5) / 2) * (int((-1 + (1 + 8 * candies) ** 0.5) / 2) + 1)) // 2)) for x in range(1, num_people + 1)]
```