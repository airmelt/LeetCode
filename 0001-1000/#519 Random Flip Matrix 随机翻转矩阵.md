# 519 Random Flip Matrix 随机翻转矩阵

__Description__:
You are given the number of rows n_rows and number of columns n_cols of a 2D binary matrix where all values are initially 0. Write a function flip which chooses a 0 value uniformly at random, changes it to 1, and then returns the position [row.id, col.id] of that value. Also, write a function reset which sets all values back to 0. Try to minimize the number of calls to system's Math.random() and optimize the time and space complexity.

__Note:__

1 <= n_rows, n_cols <= 10000
0 <= row.id < n_rows and 0 <= col.id < n_cols
flip will not be called when the matrix has no 0 values left.
the total number of calls to flip and reset will not exceed 1000.

__Example:__

Example 1:

Input:
["Solution","flip","flip","flip","flip"]
[[2,3],[],[],[],[]]
Output: [null,[0,1],[1,2],[1,0],[1,1]]

Example 2:

Input:
["Solution","flip","flip","reset","flip"]
[[1,2],[],[],[],[]]
Output: [null,[0,0],[0,1],null,[0,0]]
Explanation of Input Syntax:

The input is two lists: the subroutines called and their arguments. Solution's constructor has two arguments, n_rows and n_cols. flip and reset have no arguments. Arguments are always wrapped with a list, even if there aren't any.

__题目描述__:
题中给出一个 n_rows 行 n_cols 列的二维矩阵，且所有值被初始化为 0。要求编写一个 flip 函数，均匀随机的将矩阵中的 0 变为 1，并返回该值的位置下标 [row_id,col_id]；同样编写一个 reset 函数，将所有的值都重新置为 0。尽量最少调用随机函数 Math.random()，并且优化时间和空间复杂度。

__注意:__

1 <= n_rows, n_cols <= 10000
0 <= row.id < n_rows 并且 0 <= col.id < n_cols
当矩阵中没有值为 0 时，不可以调用 flip 函数
调用 flip 和 reset 函数的次数加起来不会超过 1000 次

__示例 :__

示例 1：

输入:
["Solution","flip","flip","flip","flip"]
[[2,3],[],[],[],[]]
输出: [null,[0,1],[1,2],[1,0],[1,1]]

示例 2：

输入:
["Solution","flip","flip","reset","flip"]
[[1,2],[],[],[],[]]
输出: [null,[0,0],[0,1],null,[0,0]]
输入语法解释：

输入包含两个列表：被调用的子程序和他们的参数。Solution 的构造函数有两个参数，分别为 n_rows 和 n_cols。flip 和 reset 没有参数，参数总会以列表形式给出，哪怕该列表为空

__思路__:

将二维矩阵看作一维数组, 用对列取余和除定位
每次取出翻转之后加入到哈希表中, 保证不会出现重复
时间复杂度O(mn), 空间复杂度O(n), m表示 coins数组的长度, n表示 amount

__代码__:
__C++__:

```C++
class Solution 
{
private:
    int total, count, col;
    unordered_map<int, int> m;
public:
    Solution(int n_rows, int n_cols) 
    {
        col = n_cols;
        total = count = n_rows * n_cols;
    }
    
    vector<int> flip() 
    {
        int i = rand() % count;
        while (m.count(i)) i = m[i];
        m[i] = --count;
        return {i / col, i % col};
    }
    
    void reset() 
    {
        count = total;
        m.clear();
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(n_rows, n_cols);
 * vector<int> param_1 = obj->flip();
 * obj->reset();
 */
```

__Java__:

```Java
class Solution {

    private int total = 0, count = 0, col = 0;
    private Map<Integer, Integer> map = new HashMap<>();
    private Random random = new Random();
    
    public Solution(int n_rows, int n_cols) {
        col = n_cols;
        total = n_rows * n_cols;
        count = n_rows * n_cols;
    }
    
    public int[] flip() {
        int i = random.nextInt(10001) % count;
        while (map.containsKey(i)) i = map.get(i);
        map.put(i, --count);
        return new int[]{i / col, i % col};
    }
    
    public void reset() {
        count = total;
        map.clear();
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(n_rows, n_cols);
 * int[] param_1 = obj.flip();
 * obj.reset();
 */
```

__Python__:

```Python
class Solution:

    def __init__(self, n_rows: int, n_cols: int):
        self.total = self.count = n_rows * n_cols
        self.col = n_cols
        self.record = {}

    def flip(self) -> List[int]:
        i = random.randint(0, 10e4) % self.count
        while i in self.record:
            i = self.record[i]
        self.count -= 1
        self.record[i] = self.count
        return [i // self.col, i % self.col]

    def reset(self) -> None:
        self.count = self.total
        self.record.clear()
        


# Your Solution object will be instantiated and called as such:
# obj = Solution(n_rows, n_cols)
# param_1 = obj.flip()
# obj.reset()
```
