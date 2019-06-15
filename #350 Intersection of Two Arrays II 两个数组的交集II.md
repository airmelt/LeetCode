__Description__:
Given two arrays, write a function to compute their intersection.

**Example :**
Example 1:
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2,2]

Example 2:
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [4,9]

__Note:__

Each element in the result should appear as many times as it shows in both arrays.
The result can be in any order.

__Follow up:__

What if the given array is already sorted? How would you optimize your algorithm?
What if nums1's size is small compared to nums2's size? Which algorithm is better?
What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

__题目描述__:
给定两个数组，编写一个函数来计算它们的交集。

**示例 :**
示例 1:
输入: nums1 = [1,2,2,1], nums2 = [2,2]
输出: [2,2]

示例 2:
输入: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出: [4,9]

__说明:__

输出结果中每个元素出现的次数，应与元素在两个数组中出现的次数一致。
我们可以不考虑输出结果的顺序。

__进阶:__

如果给定的数组已经排好序呢？你将如何优化你的算法？
如果 nums1 的大小比 nums2 小很多，哪种方法更优？
如果 nums2 的元素存储在磁盘上，磁盘内存是有限的，并且你不能一次加载所有的元素到内存中，你该怎么办？

__思路__:
参考[LeetCode #349 Intersection of Two Arrays 两个数组的交集](https://www.jianshu.com/p/b47062b61b54)
利用 map存储不重复的值, 将 nums1的值和出现次数存储起来, 结果插入 nums2出现的数字
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        map<int, int> map1;
        for (int i = 0; i < nums1.size(); i++) {
            if (map1.find(nums1[i]) != map1.end()) map1[nums1[i]] = ++map1[nums1[i]];
            else map1[nums1[i]] = 1;
        }

        vector<int> result;
        for (int i = 0; i < nums2.size(); i++) {
            if (map1.find(nums2[i]) != map1.end() && map1[nums2[i]] > 0){
                result.push_back(nums2[i]);
                map1[nums2[i]] = --map1[nums2[i]];
            }
        }
        return result;
    }
};
```

__Java__:
```
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Map<Integer, Integer> map = new HashMap<Integer, Integer>(nums1.length);
        for (int i = 0; i < nums1.length; i++) {
            if (map.containsKey(nums1[i])) map.put(nums1[i], map.get(nums1[i]) + 1);
            else map.put(nums1[i], 1);
        }

        List<Integer> list = new ArrayList<Integer>(nums2.length);
        for (int i = 0; i < nums2.length; i++) {
            if (map.containsKey(nums2[i]) && map.get(nums2[i]) > 0){
                list.add(nums2[i]);
                map.put(nums2[i], map.get(nums2[i]) - 1);
            }
        }
        int count = list.size();
        int[] result = new int[count];
        for (int i = 0; i < count; i++) {
            result[i] = list.get(i);
        }
        return result;
    }
}
```

__Python__:
```
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        return list(itertools.chain(*[[i] * min(nums1.count(i), nums2.count(i)) for i in (set(nums1) & set(nums2))]))
```
