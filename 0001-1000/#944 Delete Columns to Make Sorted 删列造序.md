# 944 Delete Columns to Make Sorted 删列造序

__Description__:
We are given an array A of N lowercase letter strings, all of the same length.

Now, we may choose any set of deletion indices, and for each string, we delete all the characters in those indices.

For example, if we have an array A = ["abcdef","uvwxyz"] and deletion indices {0, 2, 3}, then the final array after deletions is ["bef", "vyz"], and the remaining columns of A are ["b","v"], ["e","y"], and ["f","z"].  (Formally, the c-th column is [A[0][c], A[1][c], ..., A[A.length-1][c]].)

Suppose we chose a set of deletion indices D such that after deletions, each remaining column in A is in non-decreasing sorted order.

Return the minimum possible value of D.length.

__Example:__

Example 1:

Input: ["cba","daf","ghi"]
Output: 1
Explanation:
After choosing D = {1}, each column ["c","d","g"] and ["a","f","i"] are in non-decreasing sorted order.
If we chose D = {}, then a column ["b","a","h"] would not be in non-decreasing sorted order.

Example 2:

Input: ["a","b"]
Output: 0
Explanation: D = {}

Example 3:

Input: ["zyx","wvu","tsr"]
Output: 3
Explanation: D = {0, 1, 2}

__Note:__

1 <= A.length <= 100
1 <= A[i].length <= 1000

__题目描述__:
给定由 N 个小写字母字符串组成的数组 A，其中每个字符串长度相等。

删除 操作的定义是：选出一组要删掉的列，删去 A 中对应列中的所有字符，形式上，第 n 列为 [A[0][n], A[1][n], ..., A[A.length-1][n]]）。

比如，有 A = ["abcdef", "uvwxyz"]，

![数组A](https://upload-images.jianshu.io/upload_images/16639143-a236bb00af604393.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

要删掉的列为 {0, 2, 3}，删除后 A 为["bef", "vyz"]， A 的列分别为["b","v"], ["e","y"], ["f","z"]。

![要删除的列](https://upload-images.jianshu.io/upload_images/16639143-3a7163b7052ce174.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

你需要选出一组要删掉的列 D，对 A 执行删除操作，使 A 中剩余的每一列都是 非降序 排列的，然后请你返回 D.length 的最小可能值。

__示例 :__

示例 1：

输入：["cba", "daf", "ghi"]
输出：1
解释：
当选择 D = {1}，删除后 A 的列为：["c","d","g"] 和 ["a","f","i"]，均为非降序排列。
若选择 D = {}，那么 A 的列 ["b","a","h"] 就不是非降序排列了。

示例 2：

输入：["a", "b"]
输出：0
解释：D = {}

示例 3：

输入：["zyx", "wvu", "tsr"]
输出：3
解释：D = {0, 1, 2}

__提示：__

1 <= A.length <= 100
1 <= A[i].length <= 1000

__思路__:

检查每一列, 如果不是有序的, 结果加 1
时间复杂度O(nm), 空间复杂度O(1), 其中 n为数组 A的大小, m为数组 A中元素的大小

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minDeletionSize(vector<string>& A) 
    {
        int result = 0;
        for (int j = 0; j < A[0].size(); j++) for (int i = 1; i < A.size(); i++)
        {
            if (A[i][j] < A[i - 1][j])
            {
                result++;
                break;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minDeletionSize(String[] A) {
        int result = 0;
        for (int j = 0; j < A[0].length(); j++) {
            for (int i = 1; i < A.length; i++) {
                if (A[i].charAt(j) < A[i - 1].charAt(j)) {
                    result++;
                    break;
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minDeletionSize(self, A: List[str]) -> int:
        return len([1 for col in zip(*A) if sorted(col) != list(col)])
```
