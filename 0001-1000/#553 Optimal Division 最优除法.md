# 553 Optimal Division 最优除法

__Description__:
Given a list of positive integers, the adjacent integers will perform the float division. For example, [2,3,4] -> 2 / 3 / 4.

However, you can add any number of parenthesis at any position to change the priority of operations. You should find out how to add parenthesis to get the maximum result, and return the corresponding expression in string format. Your expression should NOT contain redundant parenthesis.

__Example:__

Input: [1000,100,10,2]
Output: "1000/(100/10/2)"
Explanation:
1000/(100/10/2) = 1000/((100/10)/2) = 200
However, the bold parenthesis in "1000/((100/10)/2)" are redundant,
since they don't influence the operation priority. So you should return "1000/(100/10/2)".

Other cases:
1000/(100/10)/2 = 50
1000/(100/(10/2)) = 50
1000/100/10/2 = 0.5
1000/100/(10/2) = 2

__Note:__

The length of the input array is [1, 10].
Elements in the given array will be in range [2, 1000].
There is only one optimal division for each test case.

__题目描述__:
给定一组正整数，相邻的整数之间将会进行浮点除法操作。例如， [2,3,4] -> 2 / 3 / 4 。

但是，你可以在任意位置添加任意数目的括号，来改变算数的优先级。你需要找出怎么添加括号，才能得到最大的结果，并且返回相应的字符串格式的表达式。你的表达式不应该含有冗余的括号。

__示例 :__

输入: [1000,100,10,2]
输出: "1000/(100/10/2)"
解释:
1000/(100/10/2) = 1000/((100/10)/2) = 200
但是，以下加粗的括号 "1000/((100/10)/2)" 是冗余的，
因为他们并不影响操作的优先级，所以你需要返回 "1000/(100/10/2)"。

其他用例:
1000/(100/10)/2 = 50
1000/(100/(10/2)) = 50
1000/100/10/2 = 0.5
1000/100/(10/2) = 2

__说明:__

输入数组的长度在 [1, 10] 之间。
数组中每个元素的大小都在 [2, 1000] 之间。
每个测试用例只有一个最优除法解。

__思路__:

注意到数组中的数全部为正数
先做 n == 1 和 n == 2 的特殊情况
把第二个开始的数用括号和除号分割开就行
时间复杂度 O(n), 空间复杂度 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string optimalDivision(vector<int>& nums) 
    {
        string result = "";
        for (int i = 0; i < nums.size(); i++) result += to_string(nums[i]) + '/';
        return nums.size() < 3 ? result.substr(0, result.size() - 1) : result.substr(0, result.size() - 1).insert(result.find('/') + 1, 1, '(') + ')';
    }
};
```

__Java__:

```Java
class Solution {
    public String optimalDivision(int[] nums) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < nums.length; i++) sb.append(nums[i]).append("/");
        return nums.length < 3 ? sb.deleteCharAt(sb.length() - 1).toString() : sb.deleteCharAt(sb.length() - 1).insert(sb.indexOf("/") + 1, "(").append(")").toString();
    }
}
```

__Python__:

```Python
class Solution:
    def optimalDivision(self, nums: List[int]) -> str:
        return str(nums[0]) if len(nums) == 1 else str(nums[0]) + '/(' + '/'.join(list(map(str, nums[1:]))) + ')' if len(nums) > 2 else str(nums[0]) + '/' + str(nums[-1])
```
