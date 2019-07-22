__Description__:
Given scores of N athletes, find their relative ranks and the people with the top three highest scores, who will be awarded medals: "Gold Medal", "Silver Medal" and "Bronze Medal".

__Example:__
Example 1:
Input: [5, 4, 3, 2, 1]
Output: ["Gold Medal", "Silver Medal", "Bronze Medal", "4", "5"]
Explanation: The first three athletes got the top three highest scores, so they got "Gold Medal", "Silver Medal" and "Bronze Medal".
For the left two athletes, you just need to output their relative ranks according to their scores.

__Note:__
N is a positive integer and won't exceed 10,000.
All the scores of athletes are guaranteed to be unique.

__题目描述__:
给出 N 名运动员的成绩，找出他们的相对名次并授予前三名对应的奖牌。前三名运动员将会被分别授予 “金牌”，“银牌” 和“ 铜牌”（"Gold Medal", "Silver Medal", "Bronze Medal"）。

(注：分数越高的选手，排名越靠前。)

__示例：__
示例 1:

输入: [5, 4, 3, 2, 1]
输出: ["Gold Medal", "Silver Medal", "Bronze Medal", "4", "5"]
解释: 前三名运动员的成绩为前三高的，因此将会分别被授予 “金牌”，“银牌”和“铜牌” ("Gold Medal", "Silver Medal" and "Bronze Medal").
余下的两名运动员，我们只需要通过他们的成绩计算将其相对名次即可。

__提示:__

N 是一个正整数并且不会超过 10000。
所有运动员的成绩都不相同。

__思路__:
1. 排序, 把前三名转化成奖牌字符串
时间复杂度O(nlgn), 空间复杂度O(n)
2. 空间换时间, 将排名下标存在一个数组中
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```
class Solution {
public:
    vector<string> findRelativeRanks(vector<int>& nums) {
        vector<string> result(nums.size());
        int max_nums = 0, flag = 1;
        for (int i = 0; i < nums.size(); i++) max_nums = max(max_nums, nums[i]);
        int temp[max_nums + 1] = {0};
        for (int i = 0; i < nums.size(); i++) temp[nums[i]] = i + 1;
        for (int i = max_nums; i >= 0; i--) {
            if (temp[i] > 0) {
                switch (flag) {
                    case 1:
                        result[temp[i] - 1] = "Gold Medal";
                        break;
                    case 2:
                        result[temp[i] - 1] = "Silver Medal";
                        break;
                    case 3:
                        result[temp[i] - 1] = "Bronze Medal";
                        break;
                    default:
                        result[temp[i] - 1] = to_string(flag);
                }
                flag++;
            }
        }
        return result;
    }
};
```

__Java__:
```
class Solution {
    public String[] findRelativeRanks(int[] nums) {
        String[] result = new String[nums.length];
        int max = 0, flag = 1;
        for (int i = 0; i < nums.length; i++) max = Math.max(max, nums[i]);
        int[] temp = new int[max + 1];
        for (int i = 0; i < nums.length; i++) temp[nums[i]] = i + 1;
        for (int i = max; i >= 0; i--) {
            if (temp[i] > 0) {
                switch (flag) {
                    case 1:
                        result[temp[i] - 1] = "Gold Medal";
                        break;
                    case 2:
                        result[temp[i] - 1] = "Silver Medal";
                        break;
                    case 3:
                        result[temp[i] - 1] = "Bronze Medal";
                        break;
                    default:
                        result[temp[i] - 1] = Integer.toString(flag);
                }
                flag++;
            }
        }
        return result;
    }
}
```

__Python__:
```
class Solution:
    def findRelativeRanks(self, nums: List[int]) -> List[str]:
        rank = {r: i for i, r in enumerate(sorted(nums, reverse=True))}
        rank_map = {1: 'Gold Medal', 2: 'Silver Medal', 3: 'Bronze Medal'}
        return [rank_map.get(rank[num] + 1, str(rank[num] + 1)) for num in nums]
```
