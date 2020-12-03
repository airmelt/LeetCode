__Description__:
Given an integer n, return 1 - n in lexicographical order.

__Example:__
For example, given 13, return: [1,10,11,12,13,2,3,4,5,6,7,8,9].

Please optimize your algorithm to use less time and space. The input size may be as large as 5,000,000.

__题目描述__:
给定一个整数 n, 返回从 1 到 n 的字典顺序。

__示例 :__

给定 n =1 3，返回 [1,10,11,12,13,2,3,4,5,6,7,8,9] 。

请尽可能的优化算法的时间复杂度和空间复杂度。 输入的数据 n 小于等于 5,000,000。

__思路__:
1. 先排序字符串, 然后将排序好的字符串转换为整数
时间复杂度O(nlgn), 空间复杂度O(n)
2. 设置一个指针
比如需要放 1-130
指针开始指向 1, 下一个应该放 10, 所以比较指针 * 10和 n的大小关系
指针自乘 10直到超过 n
这时已经放入了 [1, 10, 100],
由于 100 * 10 > 130, 此时下一个应该是 11, 这时指针需要从 100移动到 11, 比较指针和 n的大小, 指针超过 n就自除 10, 然后自增直到 20, 下一个数为2, 所以当低位是 0的时候, 需要去掉
时间复杂度O(nlgn), 空间复杂度O(1)


__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<int> lexicalOrder(int n) 
    {
        vector<int> result(n);
        int cur = 1;
        for (int i = 0; i < n; i++)
        {
            result[i] = cur;
            if (cur * 10 <= n) cur *= 10;
            else
            {
                if (cur >= n) cur /= 10;
                ++cur;
                while (!(cur % 10)) cur /= 10;
            }
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public List<Integer> lexicalOrder(int n) {
        String list[] = new String[n];
        for (int i = 1; i < n + 1; i++) list[i - 1] = String.valueOf(i);
        Arrays.sort(list);
        List<Integer> result = new ArrayList<>(n);
        for (String s : list) result.add(Integer.valueOf(s));
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def lexicalOrder(self, n: int) -> List[int]:
        return sorted(range(1, n + 1), key=str)
```