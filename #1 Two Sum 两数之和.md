__题目描述__:
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那__两个__整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

__示例__:

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]

__Description__:
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

__Example__:

Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].

__思路__:
1. 暴力法: 对每个数, 遍历剩下的数求是否存在解. 时间复杂度O(n^2), 空间复杂度O(1)
2. 一次遍历哈希法: 建立一个map存放相应元素对应的目标元素, 并在插入的时候检查, 时间复杂度O(n), 空间复杂度O(n)

__代码__:
_C++_:
```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> result;
        // key为nums的值, value为nums下标
        map<int, int> int_map;
        for (int i = 0; i < nums.size(); i++) {
            // map.count()测试主键是否存在, 若存在返回1
            if (int_map.count(nums[i]) != 0) {
                // push_back(elem)在容器最后位置添加一个元素elem
                result.push_back(int_map[nums[i]]);
                result.push_back(i);
                break;
            }
            int_map[target- nums[i]] = i;
        }
        return result;     
    }
};

```

_Java_:
```
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        int[] result = new int[2];
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement)) {
                result[0] = map.get(complement);
                result[1] = i;
                return result;
            }
            map.put(nums[i], i);
        }
        return result;
    }
}
```

_Python_:
```
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        dic = {}
        for index, num in enumerate(nums):
            complement = target - num
            if complement in dic:
                return [dic[complement], index]
            dic[num] = index
```
