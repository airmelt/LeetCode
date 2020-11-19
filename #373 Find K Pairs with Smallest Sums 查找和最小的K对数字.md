__Description__:
You are given two integer arrays nums1 and nums2 sorted in ascending order and an integer k.

Define a pair (u,v) which consists of one element from the first array and one element from the second array.

Find the k pairs (u1,v1),(u2,v2) ...(uk,vk) with the smallest sums.

__Example:__
Example 1:

Input: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
Output: [[1,2],[1,4],[1,6]] 
Explanation: The first 3 pairs are returned from the sequence: 
             [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]

Example 2:

Input: nums1 = [1,1,2], nums2 = [1,2,3], k = 2
Output: [1,1],[1,1]
Explanation: The first 2 pairs are returned from the sequence: 
             [1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]

Example 3:

Input: nums1 = [1,2], nums2 = [3], k = 3
Output: [1,3],[2,3]
Explanation: All possible pairs are returned from the sequence: [1,3],[2,3]

__题目描述__:
给定两个以升序排列的整形数组 nums1 和 nums2, 以及一个整数 k。

定义一对值 (u,v)，其中第一个元素来自 nums1，第二个元素来自 nums2。

找到和最小的 k 对数字 (u1,v1), (u2,v2) ... (uk,vk)。

__示例 :__
示例 1:

输入: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
输出: [1,2],[1,4],[1,6]
解释: 返回序列中的前 3 对数：
     [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]

示例 2:

输入: nums1 = [1,1,2], nums2 = [1,2,3], k = 2
输出: [1,1],[1,1]
解释: 返回序列中的前 2 对数：
     [1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]

示例 3:

输入: nums1 = [1,2], nums2 = [3], k = 3 
输出: [1,3],[2,3]
解释: 也可能序列中所有的数对都被返回:[1,3],[2,3]

__思路__:
求前 k个一般使用堆
使用小顶堆, 固定 nums1中的数, 找到 nums2中的数加入结果中
时间复杂度O((k + n)lgn), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<vector<int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) 
    {
        vector<vector<int>> result;
        if (nums1.empty() or nums2.empty()) return result;
        int n = nums1.size(), m = nums2.size();
        k = min(n * m, k);
        priority_queue<pair<int, pair<int, int>>> heap;
        for (int i = 0; i < n; i++) heap.push({-nums1[i] - nums2[0], {i, 0}});
        while (result.size() < k) 
        {
            auto cur = heap.top();
            heap.pop();
            int i = cur.second.first, j = cur.second.second;
            result.push_back({nums1[i], nums2[j++]});
            if (j < m) heap.push({-nums1[i] - nums2[j], {i, j}});
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        PriorityQueue<int[]> queue = new PriorityQueue<>((o1, o2) -> compare(o2, o1));
        for (int i : nums1) {
            if (queue.size() == k && compare(queue.peek(), new int[]{i, nums2[0]}) < 0) break;
            for (int j : nums2) {
                int[] arr = new int[]{i, j};
                if (queue.size() < k) queue.offer(arr);
                else if (!queue.isEmpty() && compare(queue.peek(), arr) > 0) {
                    queue.poll();
                    queue.offer(arr);
                } else break;
            }
        }
        List<List<Integer>> result = new ArrayList<>();
        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            result.add(0, Arrays.asList(cur[0], cur[1]));
        }
        return result;
    }
    
    private int compare(int[] arr1, int[] arr2) {
        return (arr1[0] + arr1[1]) - (arr2[0] + arr2[1]);
    }
}
```

__Python__:
```Python
class Solution:
    def kSmallestPairs(self, nums1: List[int], nums2: List[int], k: int) -> List[List[int]]:
        return map(list, heapq.nsmallest(k, itertools.product(nums1, nums2), key=sum))
```