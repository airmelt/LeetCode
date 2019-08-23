__Description__:
The set S originally contains numbers from 1 to n. But unfortunately, due to the data error, one of the numbers in the set got duplicated to another number in the set, which results in repetition of one number and loss of another number.

Given an array nums representing the data status of this set after the error. Your task is to firstly find the number occurs twice and then find the number that is missing. Return them in the form of an array.

__Example:__
Example 1:

Input: nums = [1,2,2,4]
Output: [2,3]

__Note:__

The given array size will in the range [2, 10000].
The given array's numbers won't have any order.

__题目描述__:
集合 S 包含从1到 n 的整数。不幸的是，因为数据错误，导致集合里面某一个元素复制了成了集合里面的另外一个元素的值，导致集合丢失了一个整数并且有一个元素重复。

给定一个数组 nums 代表了集合 S 发生错误后的结果。你的任务是首先寻找到重复出现的整数，再找到丢失的整数，将它们以数组的形式返回。

__示例 :__
示例 1:

输入: nums = [1,2,2,4]
输出: [2,3]

__注意：__

给定数组的长度范围是 [2, 10000]。
给定的数组是无序的。

__思路__:
参考[LeetCode #268 Missing Number 缺失数字](https://www.jianshu.com/p/fc35b043855f)
可以将下标对应的数组元素取负数来判断元素是否出现过
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        int sum = 0, total = nums.size() * (nums.size() + 1) / 2;
        vector<int> result{0, 0};
        for (int num : nums) {
            num = abs(num);
            sum += num;
            if (nums[num - 1] < 0) result[0] = num;
            else nums[num - 1] *= -1;
        }
        result[1] = result[0] - sum + total;
        return result;
    }
};
```

__Java__:
```
class Solution {
    public int[] findErrorNums(int[] nums) {
        int sum = 0, total = nums.length * (nums.length + 1) / 2, result[] = new int[2];
        for (int num : nums) {
            num = Math.abs(num);
            sum += num;
            if (nums[num - 1] < 0) result[0] = num;
            else nums[num - 1] *= -1;
        }
        result[1] = result[0] - sum + total;
        return result;
    }
}
```

__Python__:
```
class Solution:
    def findErrorNums(self, nums: List[int]) -> List[int]:
        return [sum(nums) - sum(set(nums)), len(nums) * (len(nums) + 1) // 2 - sum(set(nums))]
```