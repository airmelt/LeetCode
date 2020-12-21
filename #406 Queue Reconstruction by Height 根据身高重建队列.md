__Description__:
You are given an array of people, people, which are the attributes of some people in a queue (not necessarily in order). Each people[i] = [hi, ki] represents the ith person of height hi with exactly ki other people in front who have a height greater than or equal to hi.

Reconstruct and return the queue that is represented by the input array people. The returned queue should be formatted as an array queue, where queue[j] = [hj, kj] is the attributes of the jth person in the queue (queue[0] is the person at the front of the queue).

__Example:__
Example 1:

Input: people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
Output: [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]
Explanation:
Person 0 has height 5 with no other people taller or the same height in front.
Person 1 has height 7 with no other people taller or the same height in front.
Person 2 has height 5 with two persons taller or the same height in front, which is person 0 and 1.
Person 3 has height 6 with one person taller or the same height in front, which is person 1.
Person 4 has height 4 with four people taller or the same height in front, which are people 0, 1, 2, and 3.
Person 5 has height 7 with one person taller or the same height in front, which is person 1.
Hence [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]] is the reconstructed queue.

Example 2:

Input: people = [[6,0],[5,0],[4,0],[3,2],[2,2],[1,4]]
Output: [[4,0],[5,0],[2,2],[3,2],[1,4],[6,0]]
 
__Constraints:__

1 <= people.length <= 2000
0 <= hi <= 10^6
0 <= ki < people.length
It is guaranteed that the queue can be reconstructed.

__题目描述__:
假设有打乱顺序的一群人站成一个队列，数组 people 表示队列中一些人的属性（不一定按顺序）。每个 people[i] = [hi, ki] 表示第 i 个人的身高为 hi ，前面 正好 有 ki 个身高大于或等于 hi 的人。

请你重新构造并返回输入数组 people 所表示的队列。返回的队列应该格式化为数组 queue ，其中 queue[j] = [hj, kj] 是队列中第 j 个人的属性（queue[0] 是排在队列前面的人）。

__示例 :__
示例 1：

输入：people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
输出：[[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]
解释：
编号为 0 的人身高为 5 ，没有身高更高或者相同的人排在他前面。
编号为 1 的人身高为 7 ，没有身高更高或者相同的人排在他前面。
编号为 2 的人身高为 5 ，有 2 个身高更高或者相同的人排在他前面，即编号为 0 和 1 的人。
编号为 3 的人身高为 6 ，有 1 个身高更高或者相同的人排在他前面，即编号为 1 的人。
编号为 4 的人身高为 4 ，有 4 个身高更高或者相同的人排在他前面，即编号为 0、1、2、3 的人。
编号为 5 的人身高为 7 ，有 1 个身高更高或者相同的人排在他前面，即编号为 1 的人。
因此 [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]] 是重新构造后的队列。

示例 2：

输入：people = [[6,0],[5,0],[4,0],[3,2],[2,2],[1,4]]
输出：[[4,0],[5,0],[2,2],[3,2],[1,4],[6,0]]
 
__提示：__

1 <= people.length <= 2000
0 <= hi <= 10^6
0 <= ki < people.length
题目数据确保队列可以被重建

__思路__:
先按照 k排序, 再按照身高逆序排序
然后按 k插入数组
时间复杂度O(nlgn), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) 
    {
        vector<vector<int>> result;
        sort(people.begin(), people.end(), [](const vector<int> &a, const vector<int> &b){ return a[0] == b[0] ? a[1] < b[1] : a[0] > b[0]; });
        for (auto &p : people) 
        {
            if (p[1] >= result.size()) result.emplace_back(p);
            else result.insert(result.begin() + p[1], p);
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        List<int[]> list = new ArrayList<>();
        Arrays.sort(people, (int[] a, int[] b) -> (a[0] == b[0] ? a[1] - b[1] : b[0] - a[0]));
        for (int[] p : people) list.add(p[1], p);
        return list.toArray(new int[list.size()][]);
    }
}
```

__Python__:
```Python
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        people.sort(key=lambda x:(-x[0], x[1]))
        result = []
        for p in people:
            result.insert(p[1], p)
        return result
```