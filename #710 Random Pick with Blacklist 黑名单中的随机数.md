# 710 Random Pick with Blacklist 黑名单中的随机数

__Description__:
Given a blacklist blacklist containing unique integers from [0, n), write a function to return a uniform random integer from [0, n) which is NOT in blacklist.

Optimize it such that it minimizes the call to system’s Math.random().

__Note:__

1 <= n <= 1000000000
0 <= blacklist.length < min(100000, n)
[0, n) does NOT include n. See interval notation.

__Example:__

Example 1:

Input:
["Solution","pick","pick","pick"]
[[1,[]],[],[],[]]
Output: [null,0,0,0]

Example 2:

Input:
["Solution","pick","pick","pick"]
[[2,[]],[],[],[]]
Output: [null,1,1,1]

Example 3:

Input:
["Solution","pick","pick","pick"]
[[3,[1]],[],[],[]]
Output: [null,0,0,2]

Example 4:

Input:
["Solution","pick","pick","pick"]
[[4,[2]],[],[],[]]
Output: [null,1,3,1]

__Explanation of Input Syntax:__

The input is two lists: the subroutines called and their arguments. Solution's constructor has two arguments, n and the blacklist blacklist. pick has no arguments. Arguments are always wrapped with a list, even if there aren't any.

__题目描述__:
给定一个包含 [0，n) 中不重复整数的黑名单 blacklist ，写一个函数从 [0, n) 中返回一个不在 blacklist 中的随机整数。

对它进行优化使其尽量少调用系统方法 Math.random() 。

__提示:__

1 <= n <= 1000000000
0 <= blacklist.length < min(100000, N)
[0, n) 不包含 n ，详细参见 interval notation 。

__示例 :__

示例 1：

输入：
["Solution","pick","pick","pick"]
[[1,[]],[],[],[]]
输出：[null,0,0,0]

示例 2：

输入：
["Solution","pick","pick","pick"]
[[2,[]],[],[],[]]
输出：[null,1,1,1]

示例 3：

输入：
["Solution","pick","pick","pick"]
[[3,[1]],[],[],[]]
输出：[null,0,0,2]

示例 4：

输入：
["Solution","pick","pick","pick"]
[[4,[2]],[],[],[]]
输出：[null,1,3,1]

__输入语法说明：__

输入是两个列表：调用成员函数名和调用的参数。Solution的构造函数有两个参数，n 和黑名单 blacklist。pick 没有参数，输入参数是一个列表，即使参数为空，也会输入一个 [] 空列表。

__思路__:

假设黑名单集中在后半部
那么每次只需要在前半部随机选取一个元素即可
那么如果将前半部中有黑名单的元素和后半部的白名单元素建立一个一一对应的关系
则每次取到黑名单元素的时候就映射到白名单元素上即可
时间复杂度为平均 O(lgn), 最差为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    Solution(int n, vector<int>& blacklist) 
    {
        sort(blacklist.begin(), blacklist.end());
        set<int> s(blacklist.begin(), blacklist.end());
        count = n - blacklist.size();
        for (int i = 0, j = count; i < blacklist.size(); i++) 
        {
            if (blacklist[i] < count) 
            {
                while (s.count(j)) ++j;
                m[blacklist[i]] = j++;
            }
        }
    }
    
    int pick() 
    {
        int r = rand() % count;
        return m.count(r) ? m[r] : r; 
    }
private:
    map<int, int> m;
    int count;
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(n, blacklist);
 * int param_1 = obj->pick();
 */
```

__Java__:

```Java
class Solution {

    private Map<Integer, Integer> map;
    private int count;
    private Random random;
    
    public Solution(int n, int[] blacklist) {
        random = new Random();
        map = new HashMap<>();
        Arrays.sort(blacklist);
        Set<Integer> set = new HashSet();
        for (int num : blacklist) set.add(num);
        count = n - blacklist.length;
        for (int i = 0, j = count; i < blacklist.length; i++) {
            if (blacklist[i] < count) {
                while (set.contains(j)) ++j;
                map.put(blacklist[i], j++);
            }
        }
    }
    
    public int pick() {
        int r = random.nextInt(count);
        return map.containsKey(r) ? map.get(r) : r;
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(n, blacklist);
 * int param_1 = obj.pick();
 */
```

__Python__:

```Python
class Solution:

    def __init__(self, n: int, blacklist: List[int]):
        self.count = n - len(blacklist)
        self.m = dict(zip({i for i in blacklist if i < self.count}, {*range(self.count, n)} - set(blacklist)))

    def pick(self) -> int:
        return r if (r := randint(0, self.count - 1)) not in self.m else self.m[r]


# Your Solution object will be instantiated and called as such:
# obj = Solution(n, blacklist)
# param_1 = obj.pick()
```
