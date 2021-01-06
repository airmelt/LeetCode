# 315 Count of Smaller Numbers After Self 计算右侧小于当前元素的个数

__Description__:
You are given an integer array nums and you have to return a new counts array. The counts array has the property where counts[i] is the number of smaller elements to the right of nums[i].

__Example:__

Example 1:

Input: nums = [5,2,6,1]
Output: [2,1,1,0]
Explanation:
To the right of 5 there are 2 smaller elements (2 and 1).
To the right of 2 there is only 1 smaller element (1).
To the right of 6 there is 1 smaller element (1).
To the right of 1 there is 0 smaller element.

__Constraints:__

0 <= nums.length <= 10^5
-10^4 <= nums[i] <= 10^4

__题目描述__:
给定一个整数数组 nums，按要求返回一个新数组 counts。数组 counts 有该性质： counts[i] 的值是  nums[i] 右侧小于 nums[i] 的元素的数量。

__示例 :__

输入：nums = [5,2,6,1]
输出：[2,1,1,0]
解释：
5 的右侧有 2 个更小的元素 (2 和 1)
2 的右侧仅有 1 个更小的元素 (1)
6 的右侧有 1 个更小的元素 (1)
1 的右侧有 0 个更小的元素

__提示：__

0 <= nums.length <= 10^5
-10^4 <= nums[i] <= 10^4

__思路__:

1. 插入排序
每次用二分查找查找位置, 再插入临时数组
时间复杂度O(n ^ 2), 空间复杂度O(n), 插入需要 O(n)的时间复杂度
2. 归并排序
每次找到数字归并排序的位置
可以用一个下标数组记录
时间复杂度O(nlgn), 空间复杂度O(n)
3. 树状数组
lowbit(int x) -> x & (-x)用来返回 x的最后一位 1
类似海明码的操作
时间复杂度O(nlgn), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    int lowbit(int x) 
    {
        return x & (-x);
    }
    
    void update(int i, vector<int> &C)
    {
        while (i < C.size()) 
        {
            ++C[i];
            i += lowbit(i);
        }
    }
    
    void query (int i, int j, vector<int> &C, vector<int> &result)
    {
        while (i >= 1) 
        {
            result[j] += C[i];
            i -= lowbit(i);
        }
    }
public:
    vector<int> countSmaller(vector<int>& nums) 
    {
        vector<int> result(nums.size(), 0);
        unordered_set<int> s(nums.begin(), nums.end());
        vector<int> N(s.begin(), s.end());
        sort(N.begin(), N.end());
        map<int, int> m;
        for (int j = 1; j < 1 + N.size(); ++j) m[N[j - 1]] = j;
        int i;
        vector<int> C(N.size() + 1, 0);
        for (int j = nums.size() - 1; j >= 0; --j) 
        {
            i = m[nums[j]];
            update(i, C);
            if (i != 1) query(i - 1, j, C, result);
            else result[j] = 0;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {

    private int[] index;
    private int[] aux;
    private int[] counter;

    public List<Integer> countSmaller(int[] nums) {
        List<Integer> result = new ArrayList<>();
        int len = nums.length;
        if (len == 0) return result;
        aux = new int[len];
        counter = new int[len];
        index = new int[len];
        for (int i = 0; i < len; i++) index[i] = i;
        mergeAndCount(nums, 0, len - 1);
        for (int i = 0; i < len; i++) result.add(counter[i]);
        return result;
    }
    
    private void mergeAndCount(int[] nums, int l, int r) {
        if (l == r) return;
        int m = l + ((r - l) >> 1);
        mergeAndCount(nums, l, m);
        mergeAndCount(nums, m + 1, r);
        if (nums[index[m]] > nums[index[m + 1]]) sortAndCount(nums, l, m, r);
    }
    
    private void sortAndCount(int[] nums, int l, int m, int r) {
        for (int i = l; i <= r; i++) aux[i] = index[i];
        int i = l, j = m + 1;
        for (int k = l; k <= r; k++) {
            if (i > m) {
                index[k] = aux[j++];
            } else if (j > r) {
                index[k] = aux[i++];
                counter[index[k]] += (r - m);
            } else if (nums[aux[i]] <= nums[aux[j]]) {
                index[k] = aux[i++];
                counter[index[k]] += (j - m - 1);
            } else index[k] = aux[j++];
        }
    }
}
```

__Python__:

```Python
class Solution:
    def countSmaller(self, nums: List[int]) -> List[int]:
        queue, result = [], []
        for num in nums[::-1]:
            i = bisect.bisect_left(queue, num)
            result.append(i)
            queue.insert(i, num)
        return result[::-1]
```
