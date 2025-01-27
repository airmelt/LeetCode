# 2424 Longest Uploaded Prefix 最长上传前缀

__Description:__

You are given a stream of `n` videos, each represented by a __distinct__ number from `1` to `n` that you need to "upload" to a server. You need to implement a data structure that calculates the length of the __longest uploaded prefix__ at various points in the upload process.

We consider `i` to be an uploaded prefix if all videos in the range `1` to `i` (__inclusive__) have been uploaded to the server. The longest uploaded prefix is the __maximum__ value of `i` that satisfies this definition.

Implement the `LUPrefix` class:

- `LUPrefix(int n)` Initializes the object for a stream of `n` videos.
- `void upload(int video)` Uploads `video` to the server.
- `int longest()` Returns the length of the __longest uploaded prefix__ defined above.

__Example:__

Example 1:

```text
Input
["LUPrefix", "upload", "longest", "upload", "longest", "upload", "longest"]
[[4], [3], [], [1], [], [2], []]
Output
[null, null, 0, null, 1, null, 3]

Explanation
```

```Java
LUPrefix server = new LUPrefix(4);   // Initialize a stream of 4 videos.
server.upload(3);                    // Upload video 3.
server.longest();                    // Since video 1 has not been uploaded yet, there is no prefix.
                                     // So, we return 0.
server.upload(1);                    // Upload video 1.
server.longest();                    // The prefix [1] is the longest uploaded prefix, so we return 1.
server.upload(2);                    // Upload video 2.
server.longest();                    // The prefix [1,2,3] is the longest uploaded prefix, so we return 3.
```

__Constraints:__

- `1 <= n <= 10 ^ 5`
- `1 <= video <= n`
- All values of `video` are __distinct__.
- At most `2 * 10 ^ 5` calls __in total__ will be made to `upload` and `longest`.
- At least one call will be made to `longest`.

__题目描述:__

给你一个 `n` 个视频的上传序列，每个视频编号为 `1` 到 `n` 之间的 __不同__ 数字，你需要依次将这些视频上传到服务器。请你实现一个数据结构，在上传的过程中计算 __最长上传前缀__ 。

如果 __闭区间__ `1` 到 `i` 之间的视频全部都已经被上传到服务器，那么我们称 `i` 是上传前缀。最长上传前缀指的是符合定义的 `i` 中的 __最大值__ 。

请你实现 `LUPrefix` 类:

- `LUPrefix(int n)` 初始化一个 `n` 个视频的流对象。
- `void upload(int video)` 上传 `video` 到服务器。
- `int longest()` 返回上述定义的 __最长上传前缀__ 的长度。

__示例:__

示例 1：

```text
输入：
["LUPrefix", "upload", "longest", "upload", "longest", "upload", "longest"]
[[4], [3], [], [1], [], [2], []]
输出：
[null, null, 0, null, 1, null, 3]

解释：
```

```Java
LUPrefix server = new LUPrefix(4);   // 初始化 4个视频的上传流
server.upload(3);                    // 上传视频 3 。
server.longest();                    // 由于视频 1 还没有被上传，最长上传前缀是 0 。
server.upload(1);                    // 上传视频 1 。
server.longest();                    // 前缀 [1] 是最长上传前缀，所以我们返回 1 。
server.upload(2);                    // 上传视频 2 。
server.longest();                    // 前缀 [1,2,3] 是最长上传前缀，所以我们返回 3 。
```

__提示：__

- `1 <= n <= 10 ^ 5`
- `1 <= video <= 10 ^ 5`
- `video` 中所有值 __互不相同__ 。
- `upload` 和 `longest` __总调用__ 次数至多不超过 `2 * 10 ^ 5` 次。
- 至少会调用 `longest` 一次。

__思路:__

```text
哈希表
因为上传的前缀不会减少
用哈希表记录所有已经上传的视频
每次上传视频时, 将视频编号加入哈希表
记录当前最长上传前缀的编号, 初始化为 1
每次查询最长上传前缀时, 如果当前编号在哈希表中, 则继续向后查找
返回当前编号 - 1
upload() 时间复杂度为 O(1), longest() 均摊时间复杂度为 O(1), 空间复杂度为 O(N), 最多有 N 个视频, 每个视频只会被访问一次
```

__代码:__

__C++__:

```C++
class LUPrefix 
{
public:
    LUPrefix(int n) {}
    
    void upload(int video) 
    {
        data.insert(video);
    }
    
    int longest() 
    {
        while (data.count(cur)) ++cur;
        return cur - 1;    
    }
private:
    unordered_set<int> data;
    int cur = 1;
};

/**
 * Your LUPrefix object will be instantiated and called as such:
 * LUPrefix* obj = new LUPrefix(n);
 * obj->upload(video);
 * int param_2 = obj->longest();
 */
```

__Java__:

```Java
class LUPrefix {
    private Set<Integer> data = new HashSet<>();
    private int cur = 1;

    public LUPrefix(int n) {}
    
    public void upload(int video) {
        data.add(video);
    }
    
    public int longest() {
        while (data.contains(cur)) ++cur;
        return cur - 1;
    }
}

/**
 * Your LUPrefix object will be instantiated and called as such:
 * LUPrefix obj = new LUPrefix(n);
 * obj.upload(video);
 * int param_2 = obj.longest();
 */
```

__Python__:

```Python
class LUPrefix:

    def __init__(self, n: int):
        self.cur = 1
        self.data = set()
        

    def upload(self, video: int) -> None:
        self.data.add(video)
        

    def longest(self) -> int:
        while self.cur in self.data:
            self.cur += 1
        return self.cur - 1
        


# Your LUPrefix object will be instantiated and called as such:
# obj = LUPrefix(n)
# obj.upload(video)
# param_2 = obj.longest()
```
