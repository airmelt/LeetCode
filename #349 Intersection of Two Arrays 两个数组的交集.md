__Description__:
Given two arrays, write a function to compute their intersection.

**Example :**
Example 1:
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]

Example 2:
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]

__Note:__

Each element in the result must be unique.
The result can be in any order.

__题目描述__:
给定两个数组，编写一个函数来计算它们的交集。

**示例 :**
示例 1:
输入: nums1 = [1,2,2,1], nums2 = [2,2]
输出: [2]

示例 2:
输入: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出: [9,4]

__说明:__

输出结果中的每个元素一定是唯一的。
我们可以不考虑输出结果的顺序。

__思路__:
利用 set存储不重复的值
1. 先将 nums1全部存入 set, 循环判断 set不在 nums2的全部移除
2. 使用 2个 set判断, 交集加入 set
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        set<int> set1(nums1.begin(), nums1.end());
        set<int> set2(nums2.begin(), nums2.end());
        vector<int> result;
        /* set_intersection(InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2, OutputIterator result) 传入两个排序 set的首尾
         * back_inserter(Iterator i) 使用 push_back()方法向 i中插入元素
         */
        set_intersection(set1.begin(), set1.end(), set2.begin(), set2.end(), back_inserter(result));
        return result;
    }
};
```

__Java__:
```
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> set = new HashSet<>();
        Set<Integer> temp = new HashSet<>();
        for (int i = 0; i < nums1.length; i++) {
            temp.add(nums1[i]);
        }
        for (int i = 0; i < nums2.length; i++) {
            if (temp.contains(nums2[i])) set.add(nums2[i]);
        }
        int[] result = new int[set.size()];
        Iterator iterator = set.iterator();
        int i = 0;
        while (iterator.hasNext()) {
            result[i++] = (int)iterator.next();
        }
        return result;
    }
}
```

__Python__:
```
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        return list(set(nums1) & set(nums2))
```
