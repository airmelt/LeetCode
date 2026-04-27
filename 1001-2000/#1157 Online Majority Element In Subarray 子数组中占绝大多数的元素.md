# 1157 Online Majority Element In Subarray 子数组中占绝大多数的元素

__Description__:
Design a data structure that efficiently finds the majority element of a given subarray.

The majority element of a subarray is an element that occurs threshold times or more in the subarray.

Implementing the MajorityChecker class:

MajorityChecker(int[] arr) Initializes the instance of the class with the given array arr.
int query(int left, int right, int threshold) returns the element in the subarray arr[left...right] that occurs at least threshold times, or -1 if no such element exists.

__Example:__

Example 1:

Input
["MajorityChecker", "query", "query", "query"]
[[[1, 1, 2, 2, 1, 1]], [0, 5, 4], [0, 3, 3], [2, 3, 2]]
Output
[null, 1, -1, 2]

Explanation

```Java
MajorityChecker majorityChecker = new MajorityChecker([1, 1, 2, 2, 1, 1]);
majorityChecker.query(0, 5, 4); // return 1
majorityChecker.query(0, 3, 3); // return -1
majorityChecker.query(2, 3, 2); // return 2
```

__Constraints:__

1 <= arr.length <= 2 \* 10^4
1 <= arr[i] <= 2 \* 10^4
0 <= left <= right < arr.length
threshold <= right - left + 1
2 \* threshold > right - left + 1
At most 10^4 calls will be made to query.

__题目描述__:
设计一个数据结构，有效地找到给定子数组的 多数元素 。

子数组的 多数元素 是在子数组中出现 threshold 次数或次数以上的元素。

实现 MajorityChecker 类:

MajorityChecker(int[] arr) 会用给定的数组 arr 对 MajorityChecker 初始化。
int query(int left, int right, int threshold) 返回子数组中的元素  arr[left...right] 至少出现 threshold 次数，如果不存在这样的元素则返回 -1。

__示例 :__

示例 1：

输入:
["MajorityChecker", "query", "query", "query"]
[[[1, 1, 2, 2, 1, 1]], [0, 5, 4], [0, 3, 3], [2, 3, 2]]
输出：
[null, 1, -1, 2]

解释：

```Java
MajorityChecker majorityChecker = new MajorityChecker([1,1,2,2,1,1]);
majorityChecker.query(0,5,4); // 返回 1
majorityChecker.query(0,3,3); // 返回 -1
majorityChecker.query(2,3,2); // 返回 2
```

__提示:__

1 <= arr.length <= 2 \* 10^4
1 <= arr[i] <= 2 \* 10^4
0 <= left <= right < arr.length
threshold <= right - left + 1
2 \* threshold > right - left + 1
调用 query 的次数最多为 10^4

__思路__:

1. 暴力法(Java)
每次开一个超过 20000 的数组直接统计出现次数
时间复杂度为 O(qn), 空间复杂度为 O(n)
2. 二分法(很可能超时)
用哈希表记录每个数字出现的下标
查找的时候用二分找到出现的位置
预处理时间复杂度为 O(n), 时间复杂度为 O(qnlgn), 空间复杂度为 O(n), n 表示数组的长度, q 表示查询的次数
3. 线段树 ➕ 摩尔投票 + 有序
线段树中端点记录的数值为最有可能的解
利用摩尔投票选择当前次数最多的节点, 并进行合并, 合并后的值即为摩尔投票值
注意到阈值一定大于查找长度的一半
配合有序哈希二分查找, 可以加快查找有可能的数值
再筛选出现次数足够的数
预处理时间复杂度为 O(n), 查询时间复杂度为 O(qlgn), 空间复杂度为 O(n), n 表示数组的长度, q 表示查询的次数

__代码__:
__C++__:

```C++
class MajorityChecker 
{
public:
    int size;
    vector<pair<int, int>> segment_tree;
    map<int, vector<int>> m;
    
    void build(int l, int r, int index, vector<int>& arr)
    {
        if (l == r) segment_tree[index] = make_pair(arr[l], 1);
        else
        {
            int mid = l + ((r - l) >> 1);
            build(l, mid, index * 2 + 1, arr);
            build(mid + 1, r, index * 2 + 2, arr);
            int l_num = segment_tree[index * 2 + 1].first, r_num = segment_tree[index * 2 + 2].first, l_count = segment_tree[index * 2 + 1].second, r_count = segment_tree[index * 2 + 2].second;
            if (l_num == r_num) segment_tree[index] = make_pair(l_num, l_count + r_count);
            else
            {
                if (l_count >= r_count) segment_tree[index] = make_pair(l_num, l_count - r_count);
                else segment_tree[index] = make_pair(r_num, r_count - l_count);
            }
        }
    }

    pair<int, int> find(int sl, int sr, int l, int r, int index)
    {
        if (sl > r or sr < l) return make_pair(1, 0);
        else if (sl <= l and sr >= r) return segment_tree[index];
        else
        {
            int mid = l + ((r - l) >> 1);
            return combine(find(sl, sr, l, mid, index * 2 + 1), find(sl, sr, mid + 1, r, index * 2 + 2));
        }
    }

    pair<int, int> combine(pair<int, int> a, pair<int, int> b)
    {
        if (a.first == b.first) return make_pair(a.first, a.second + b.second);
        else
        {
            if (a.second >= b.second) return make_pair(a.first, a.second - b.second);
            else return make_pair(b.first, b.second - a.second);
        }
    }

    MajorityChecker(vector<int>& arr) 
    {
        size = arr.size();
        segment_tree = vector<pair<int, int>> (4 * size, make_pair(0, 0));
        for (int i = 0; i < size; ++i) m[arr[i]].emplace_back(i);
        build(0, size - 1, 0, arr);
    }
    
    int query(int left, int right, int threshold) 
    {
        pair<int, int> maybe = find(left, right, 0, size - 1, 0);
        int num = maybe.first, count = upper_bound(m[num].begin(), m[num].end(), right) - lower_bound(m[num].begin(), m[num].end(), left);
        if (count < threshold) return -1;
        return num;
    }
};

/**
 * Your MajorityChecker object will be instantiated and called as such:
 * MajorityChecker* obj = new MajorityChecker(arr);
 * int param_1 = obj->query(left,right,threshold);
 */
```

__Java__:

```Java
class MajorityChecker {
    
    private int[] data;

    public MajorityChecker(int[] arr) {
        this.data = arr;
    }
    
    public int query(int left, int right, int threshold) {
        int[] counts = new int[20001];
        for (int i = left; i <= right; i++) {
            counts[data[i]]++;
            if (counts[data[i]] == threshold) return data[i];
        }
        return -1;
    }
}

/**
 * Your MajorityChecker object will be instantiated and called as such:
 * MajorityChecker obj = new MajorityChecker(arr);
 * int param_1 = obj.query(left,right,threshold);
 */
```

__Python__:

```Python
class Node:
    def __init__(self, start: int, end: int, num: int=0, count: int=0) -> None:
        self.left = None
        self.right = None
        self.start = start
        self.end = end
        self.num_count = (num, count)

class SegmentTree:
    def __init__(self, data) -> None:
        self.data = data
        self.root = self._build(0, len(self.data) - 1)

    def _build(self, s: int, t: int) -> Node:
        if s == t:
            return Node(s, t, self.data[s], 1)
        cur = Node(s, t)
        mid = s + ((t - s) >> 1)
        cur.left = self._build(s, mid)
        cur.right = self._build(mid + 1, t)
        left_num, left_count = cur.left.num_count
        right_num, right_count = cur.right.num_count
        cur.num_count = (left_num, left_count + right_count) if left_num == right_num else (right_num, right_count - left_count) if left_count <= right_count else (left_num, left_count - right_count)
        return cur

    def query(self, s: int, t: int) -> tuple:
        return self._query(self.root, s, t)

    def _query(self, node: Node, s: int, t: int) -> tuple:
        if node.start > t or node.end < s:
            return (node.num_count[0], 0)
        if node.start >= s and node.end <= t:
            return node.num_count
        left_num, left_count = self._query(node.left, s, t)
        right_num, right_count = self._query(node.right, s, t)
        return (left_num, left_count + right_count) if left_num == right_num else (right_num, right_count - left_count) if left_count <= right_count else (left_num, left_count - right_count)


class MajorityChecker:

    def __init__(self, arr: List[int]) -> None:
        self.segment_tree = SegmentTree(arr)
        self.data = defaultdict(list)
        for i in range(len(arr)):
            self.data[arr[i]].append(i)

    def query(self, left: int, right: int, threshold: int) -> int:
        num, _ = self.segment_tree.query(left, right)
        return num if bisect_right(self.data[num], right) - bisect_left(self.data[num], left) >= threshold else -1
```
