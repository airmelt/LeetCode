__Description__:
Given an array of 2n integers, your task is to group these integers into n pairs of integer, say (a1, b1), (a2, b2), ..., (an, bn) which makes sum of min(ai, bi) for all i from 1 to n as large as possible.

__Example:__
Example 1:
Input: [1,4,3,2]

Output: 4
Explanation: n is 2, and the maximum sum of pairs is 4 = min(1, 2) + min(3, 4).

__Note:__
n is a positive integer, which is in the range of [1, 10000].
All the integers in the array will be in the range of [-10000, 10000].

__题目描述__:
给定长度为 2n 的数组, 你的任务是将这些数分成 n 对, 例如 (a1, b1), (a2, b2), ..., (an, bn) ，使得从1 到 n 的 min(ai, bi) 总和最大。

__示例 :__
示例 1:

输入: [1,4,3,2]

输出: 4
解释: n 等于 2, 最大总和为 4 = min(1, 2) + min(3, 4).

__提示:__

n 是正整数,范围在 [1, 10000].
数组中的元素范围在 [-10000, 10000].

__思路__:
由于数组的元素范围已经确定, 可以用一个临时数组当作 map存储出现的数字及个数
采取基数排序的思想
只需要取排序后的位于奇数位置上的元素和即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    int arrayPairSum(vector<int>& nums) {
        int temp[20001] = {0}, index = 1, result = 0;
        for (int i : nums) temp[i + 10000]++;
        for (int i = 0; i < 20001;) {
            if (temp[i]) {
                if (index & 1) result += i - 10000;
                index++;
                temp[i]--;
            } else i++;
        }
        return result;
    }
};
```

__Java__:
```
class Solution {
    public int arrayPairSum(int[] nums) {
        int[] temp = new int[20001];
        int index = 1, result = 0;
        for (int i : nums) temp[i + 10000]++;
        for (int i = 0; i < 20001;) {
            if (temp[i] != 0) {
                if ((index & 1) != 0) result += i - 10000;
                index++;
                temp[i]--;
            } else i++;
        }
        return result;
    }
}
```

__Python__:
```
class Solution:
    def arrayPairSum(self, nums: List[int]) -> int:
        return sum(sorted(nums)[::2])
```
