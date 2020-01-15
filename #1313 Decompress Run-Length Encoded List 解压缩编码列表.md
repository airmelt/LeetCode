__Description__:
We are given a list nums of integers representing a list compressed with run-length encoding.

Consider each adjacent pair of elements [a, b] = [nums[2*i], nums[2*i+1]] (with i >= 0).  For each such pair, there are a elements with value b in the decompressed list.

Return the decompressed list.

__Example:__
Example 1:

Input: nums = [1,2,3,4]
Output: [2,4,4,4]
 
__Constraints:__

2 <= nums.length <= 100
nums.length % 2 == 0
1 <= nums[i] <= 100

__题目描述__:
给你一个以行程长度编码压缩的整数列表 nums 。

考虑每相邻两个元素 [a, b] = [nums[2*i], nums[2*i+1]] （其中 i >= 0 ），每一对都表示解压后有 a 个值为 b 的元素。

请你返回解压后的列表。

__示例 :__

输入：nums = [1,2,3,4]
输出：[2,4,4,4]
 
__提示：__

2 <= nums.length <= 100
nums.length % 2 == 0
1 <= nums[i] <= 100

__思路__:
两层循环, 第一层找到偶数下标, 这个表示后面的数字加入的次数, 第二层加入数组, 也可以先计算出最后数组的大小
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<int> decompressRLElist(vector<int>& nums) 
    {
        vector<int> result;
        for (int i = 0; i < nums.size(); i += 2) for (int j = nums[i]; j > 0; j--) result.push_back(nums[i + 1]);
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int[] decompressRLElist(int[] nums) {
        int size = 0;
        for (int i = 0; i < nums.length; i += 2) size += nums[i];
        int result[] = new int[size], index = 0;
        for (int i = 0; i < nums.length / 2; i++) for (int j = 0; j < nums[i * 2]; j++) result[index++] = nums[i * 2 + 1];
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def decompressRLElist(self, nums: List[int]) -> List[int]:
        return itertools.chain.from_iterable([b] * a for a, b in zip(nums[::2], nums[1::2]))
```