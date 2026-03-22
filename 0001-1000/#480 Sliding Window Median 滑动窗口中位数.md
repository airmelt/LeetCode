# 480 Sliding Window Median 滑动窗口中位数

__Description__:
Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

Examples:
[2,3,4] , the median is 3

[2,3], the median is (2 + 3) / 2 = 2.5

Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position. Your job is to output the median array for each window in the original array.

__Example:__

For example,
Given nums = [1,3,-1,-3,5,3,6,7], and k = 3.

```text
Window position                Median
---------------               -----
[1  3  -1] -3  5  3  6  7       1
 1 [3  -1  -3] 5  3  6  7       -1
 1  3 [-1  -3  5] 3  6  7       -1
 1  3  -1 [-3  5  3] 6  7       3
 1  3  -1  -3 [5  3  6] 7       5
 1  3  -1  -3  5 [3  6  7]      6
```

Therefore, return the median sliding window as [1,-1,-1,3,5,6].

__Note:__
You may assume k is always valid, ie: k is always smaller than input array's size for non-empty array.
Answers within 10^-5 of the actual value will be accepted as correct.

__题目描述__:
中位数是有序序列最中间的那个数。如果序列的长度是偶数，则没有最中间的数；此时中位数是最中间的两个数的平均数。

例如：

[2,3,4]，中位数是 3
[2,3]，中位数是 (2 + 3) / 2 = 2.5
给你一个数组 nums，有一个长度为 k 的窗口从最左端滑动到最右端。窗口中有 k 个数，每次窗口向右移动 1 位。你的任务是找出每次窗口移动后得到的新窗口中元素的中位数，并输出由它们组成的数组。

__示例 :__

给出 nums = [1,3,-1,-3,5,3,6,7]，以及 k = 3。

```text
窗口位置                      中位数
---------------               -----
[1  3  -1] -3  5  3  6  7       1
 1 [3  -1  -3] 5  3  6  7      -1
 1  3 [-1  -3  5] 3  6  7      -1
 1  3  -1 [-3  5  3] 6  7       3
 1  3  -1  -3 [5  3  6] 7       5
 1  3  -1  -3  5 [3  6  7]      6
```

因此，返回该滑动窗口的中位数数组 [1,-1,-1,3,5,6]。

__提示:__

你可以假设 k 始终有效，即：k 始终小于等于输入的非空数组的元素个数。
与真实值误差在 10 ^ -5 以内的答案将被视作正确答案。

__思路__:

1. 使用插入排序每次查找使用二分查找
时间复杂度O(nklgk), 空间复杂度O(k)
2. 维护两个堆, small为大根堆, 里面的元素都是小于等于中位数
big为小根堆, 里面的元素都大于中位数
small的元素的个数比 big多 1或者两个堆的元素个数相等
这样如果 k为奇数那么 small的顶元素就是中位数
k为偶数, 那么中位数为两个堆堆顶元素的平均数
每次滑动时, 由于堆不能 O(1)的删除特定元素, 考虑用一个 balance记录, 再用一个 map记录需要删除的元素, 等待删除元素在堆顶时再删除
时间复杂度O(nlgk), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    priority_queue<int> small;
    priority_queue<int, vector<int>, greater<int>> big;
    unordered_map<int, int> m;
    
    double get(int& k)
    {
        if (k & 1) return small.top();
        else return ((long long)small.top() + big.top()) * 0.5;
    }
public:
    vector<double> medianSlidingWindow(vector<int>& nums, int k) 
    {
        for (int i = 0; i < k; i++) small.push(nums[i]);;
        for (int i = 0; i < k / 2; i++) 
        {
            big.push(small.top()); 
            small.pop();
        }
        vector<double> result{get(k)};
        for (int i = k; i < nums.size(); i++)
        {
            int balance = 0, left = nums[i - k];
            ++m[left];
            if (!small.empty() and left <= small.top()) --balance;
            else ++balance;
            if (!small.empty() and nums[i] <= small.top())
            {
                small.push(nums[i]);
                ++balance;
            }
            else
            {
                big.push(nums[i]);
                --balance;
            }
            if (balance > 0)
            {
                big.push(small.top());
                small.pop();
            }
            else if (balance < 0)
            {
                small.push(big.top());
                big.pop();
            }
            while (!small.empty() and m[small.top()] > 0)
            {
                --m[small.top()];
                small.pop();
            }
            while (!big.empty() and m[big.top()] > 0)
            {
                --m[big.top()];
                big.pop();
            }
            result.push_back(get(k));
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
   public double[] medianSlidingWindow(int[] nums, int k) {
        int index = 0, window[] = Arrays.copyOf(nums, k);
        double[] result = new double[nums.length + 1 - k];
        Arrays.sort(window);
        result[index++] = window[k >> 1] * 0.5 + window[(k - 1) >> 1] * 0.5;
        for (int i = k; i < nums.length; i++) {
            int j = biSearch(window, nums[i - k]);
            insert(window, j, nums[i]);
            result[index++] = window[k >> 1] * 0.5 + window[(k - 1) >> 1] * 0.5;
        }
        return result;
    }

    private void insert(int[] window,int index ,int target) {
        if (index == 0 || (index + 1 < window.length && target > window[index +1 ])) {
            int i = index + 1;
            while (i < window.length && window[i] < target) window[i - 1] = window[i++];
            window[i - 1] = target;
        }
        else if (index == window.length - 1 || (index > 0 && target < window[index - 1])) {
            int i = index - 1;
            while (i > -1 && window[i] > target) window[i + 1] = window[i--];
            window[i + 1] = target;
        }
        else window[index] = target;
    }

    private int biSearch(int[] window, int target) {
        int left = 0, right = window.length - 1;
        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            if (window[mid] == target) return mid;
            else if (window[mid] > target) right = mid - 1;
            else left = mid + 1;
        }
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def medianSlidingWindow(self, nums: List[int], k: int) -> List[float]:
        return [sorted(nums[i:i + k])[k >> 1] for i in range(len(nums) - k + 1)] if k & 1 else [(sorted(nums[i:i + k])[k >> 1] + sorted(nums[i:i + k])[(k - 1) >> 1]) / 2 for i in range(len(nums) - k + 1)]
```
