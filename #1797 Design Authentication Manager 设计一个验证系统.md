# 1797 Design Authentication Manager 设计一个验证系统

__Description:__

There is an authentication system that works with authentication tokens. For each session, the user will receive a new authentication token that will expire `timeToLive` seconds after the `currentTime`. If the token is renewed, the expiry time will be _extended_ to expire `timeToLive` seconds after the (potentially different) `currentTime`.

Implement the `AuthenticationManager` class:

- `AuthenticationManager(int timeToLive)` constructs the `AuthenticationManager` and sets the `timeToLive`.
- `generate(string tokenId, int currentTime)` generates a new token with the given `tokenId` at the given `currentTime` in seconds.
- `renew(string tokenId, int currentTime)` renews the __unexpired__ token with the given `tokenId` at the given `currentTime` in seconds. If there are no unexpired tokens with the given `tokenId`, the request is ignored, and nothing happens.
- `countUnexpiredTokens(int currentTime)` returns the number of __unexpired__ tokens at the given currentTime.

Note that if a token expires at time `t`, and another action happens on time `t` ( `renew` or `countUnexpiredTokens`), the expiration takes place __before__ the other actions.

__Example:__

Example 1:

![1797-1](https://assets.leetcode.com/uploads/2021/02/25/copy-of-pc68_q2.png)

```text
Input
["AuthenticationManager", " `renew`", "generate", " `countUnexpiredTokens`", "generate", " `renew`", " `renew`", " `countUnexpiredTokens`"]
[[5], ["aaa", 1], ["aaa", 2], [6], ["bbb", 7], ["aaa", 8], ["bbb", 10], [15]]
Output
[null, null, null, 1, null, null, null, 0]
```

Explanation

```Java
AuthenticationManager authenticationManager = new AuthenticationManager(5); // Constructs the AuthenticationManager with `timeToLive` = 5 seconds.
authenticationManager. `renew`("aaa", 1); // No token exists with tokenId "aaa" at time 1, so nothing happens.
authenticationManager.generate("aaa", 2); // Generates a new token with tokenId "aaa" at time 2.
authenticationManager. `countUnexpiredTokens`(6); // The token with tokenId "aaa" is the only unexpired one at time 6, so return 1.
authenticationManager.generate("bbb", 7); // Generates a new token with tokenId "bbb" at time 7.
authenticationManager. `renew`("aaa", 8); // The token with tokenId "aaa" expired at time 7, and 8 >= 7, so at time 8 the `renew` request is ignored, and nothing happens.
authenticationManager. `renew`("bbb", 10); // The token with tokenId "bbb" is unexpired at time 10, so the `renew` request is fulfilled and now the token will expire at time 15.
authenticationManager. `countUnexpiredTokens`(15); // The token with tokenId "bbb" expires at time 15, and the token with tokenId "aaa" expired at time 7, so currently no token is unexpired, so return 0.
```

__Constraints:__

- `1 <= timeToLive <= 10 ^ 8`
- `1 <= currentTime <= 10 ^ 8`
- `1 <= tokenId.length <= 5`
- `tokenId` consists only of lowercase letters.
- All calls to `generate` will contain unique values of `tokenId`.
- The values of `currentTime` across all the function calls will be __strictly increasing__.
- At most `2000` calls will be made to all functions combined.

__题目描述:__

你需要设计一个包含验证码的验证系统。每一次验证中，用户会收到一个新的验证码，这个验证码在 `currentTime` 时刻之后 `timeToLive` 秒过期。如果验证码被更新了，那么它会在 `currentTime` （可能与之前的 `currentTime` 不同）时刻延长 `timeToLive` 秒。

请你实现 `AuthenticationManager` 类:

- `AuthenticationManager(int timeToLive)` 构造 `AuthenticationManager` 并设置 `timeToLive` 参数。
- `generate(string tokenId, int currentTime)` 给定 `tokenId` ，在当前时间 `currentTime` 生成一个新的验证码。
- `renew(string tokenId, int currentTime)` 将给定 `tokenId` 且 __未过期__ 的验证码在 `currentTime` 时刻更新。如果给定 `tokenId` 对应的验证码不存在或已过期，请你忽略该操作，不会有任何更新操作发生。
- `countUnexpiredTokens(int currentTime)` 请返回在给定 `currentTime` 时刻，__未过期__ 的验证码数目。

如果一个验证码在时刻 `t` 过期，且另一个操作恰好在时刻 `t` 发生（ `renew` 或者 `countUnexpiredTokens` 操作），过期事件 __优先于__ 其他操作。

__示例:__

示例 1：

![1797-2](https://assets.leetcode.com/uploads/2021/02/25/copy-of-pc68_q2.png)

```text
输入: 
["AuthenticationManager", " `renew`", "generate", " `countUnexpiredTokens`", "generate", " `renew`", " `renew`", " `countUnexpiredTokens`"]
[[5], ["aaa", 1], ["aaa", 2], [6], ["bbb", 7], ["aaa", 8], ["bbb", 10], [15]]
输出: 
[null, null, null, 1, null, null, null, 0]
```

```Java
解释: 
AuthenticationManager authenticationManager = new AuthenticationManager(5); // 构造 AuthenticationManager ，设置 `timeToLive` = 5 秒。
authenticationManager. `renew`("aaa", 1); // 时刻 1 时，没有验证码的 tokenId 为 "aaa" ，没有验证码被更新。
authenticationManager.generate("aaa", 2); // 时刻 2 时，生成一个 tokenId 为 "aaa" 的新验证码。
authenticationManager. `countUnexpiredTokens`(6); // 时刻 6 时，只有 tokenId 为 "aaa" 的验证码未过期，所以返回 1 。
authenticationManager.generate("bbb", 7); // 时刻 7 时，生成一个 tokenId 为 "bbb" 的新验证码。
authenticationManager. `renew`("aaa", 8); // tokenId 为 "aaa" 的验证码在时刻 7 过期，且 8 >= 7 ，所以时刻 8 的renew 操作被忽略，没有验证码被更新。
authenticationManager. `renew`("bbb", 10); // tokenId 为 "bbb" 的验证码在时刻 10 没有过期，所以 renew 操作会执行，该 token 将在时刻 15 过期。
authenticationManager. `countUnexpiredTokens`(15); // tokenId 为 "bbb" 的验证码在时刻 15 过期，tokenId 为 "aaa" 的验证码在时刻 7 过期，所有验证码均已过期，所以返回 0 。
```

__提示：__

- `1 <= timeToLive <= 10 ^ 8`
- `1 <= currentTime <= 10 ^ 8`
- `1 <= tokenId.length <= 5`
- `tokenId` 只包含小写英文字母。
- 所有 `generate` 函数的调用都会包含独一无二的 `tokenId` 值。
- 所有函数调用中， `currentTime` 的值 __严格递增__ 。
- 所有函数的调用次数总共不超过 `2000` 次。

__思路:__

```text
模拟
使用哈希表记录 tokenId 对应的时间
记录时间等于当前时间加上过期时间
当需要更新的时候, 需要检查是否有 tokenId 对应的记录, 且记录时间大于当前时间
当需要计算未过期的 tokenId 时, 需要遍历哈希表, 统计记录时间大于当前时间的 tokenId 的个数
构造函数, renew() 和 countUnexpiredTokens() 时间复杂度为 O(1), countUnexpiredTokens() 的时间复杂度为O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class AuthenticationManager 
{
public:
    AuthenticationManager(int _timeToLive): timeToLive(_timeToLive) {}
    
    void generate(string tokenId, int currentTime) 
    {
        data[tokenId] = currentTime + timeToLive;
    }
    
    void renew(string tokenId, int currentTime)
    {
        if (data[tokenId] > currentTime) generate(tokenId, currentTime);
    }
    
    int countUnexpiredTokens(int currentTime) 
    {
        int result = 0;
        for (const auto& [k, v] : data) if (v > currentTime) ++result;
        return result;
    }
private:
    int timeToLive;
    unordered_map<string, int> data;
};

/**
 * Your AuthenticationManager object will be instantiated and called as such:
 * AuthenticationManager* obj = new AuthenticationManager(timeToLive);
 * obj->generate(tokenId,currentTime);
 * obj->renew(tokenId,currentTime);
 * int param_3 = obj->countUnexpiredTokens(currentTime);
 */
```

__Java__:

```Java
class AuthenticationManager {
    private int timeToLive;
    private Map<String, Integer> data;

    public AuthenticationManager(int timeToLive) {
        this.timeToLive = timeToLive;
        this.data = new HashMap<>();
    }
    
    public void generate(String tokenId, int currentTime) {
        data.put(tokenId, currentTime + timeToLive);
    }
    
    public void renew(String tokenId, int currentTime) {
        if (data.getOrDefault(tokenId, 0) > currentTime) generate(tokenId, currentTime);
    }
    
    public int countUnexpiredTokens(int currentTime) {
        int result = 0;
        for (int v : data.values()) if (v > currentTime) ++result;
        return result;
    }
}

/**
 * Your AuthenticationManager object will be instantiated and called as such:
 * AuthenticationManager obj = new AuthenticationManager(timeToLive);
 * obj.generate(tokenId,currentTime);
 * obj.renew(tokenId,currentTime);
 * int param_3 = obj.countUnexpiredTokens(currentTime);
 */
```

__Python__:

```Python
class AuthenticationManager:

    def __init__(self, timeToLive: int):
        self.timeToLive = timeToLive
        self.data = OrderedDict()


    def generate(self, tokenId: str, currentTime: int) -> None:
        self.data[tokenId] = currentTime + self.timeToLive
        
        
    def renew(self, tokenId: str, currentTime: int) -> None:
        while self.data and next(iter(self.data.values())) <= currentTime:
            self.data.popitem(last = False)
        if tokenId in self.data:
            self.data[tokenId] = currentTime + self.timeToLive
            self.data.move_to_end(tokenId)


    def countUnexpiredTokens(self, currentTime: int) -> int:
        while self.data and next(iter(self.data.values())) <= currentTime:
            self.data.popitem(last = False)
        return len(self.data)



# Your AuthenticationManager object will be instantiated and called as such:
# obj = AuthenticationManager(timeToLive)
# obj.generate(tokenId,currentTime)
# obj.renew(tokenId,currentTime)
# param_3 = obj.countUnexpiredTokens(currentTime)
```
