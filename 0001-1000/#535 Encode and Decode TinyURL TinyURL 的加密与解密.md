# 535 Encode and Decode TinyURL TinyURL 的加密与解密

__Description__:
__Note:__
This is a companion problem to the System Design problem: Design TinyURL.
TinyURL is a URL shortening service where you enter a URL such as [https://leetcode.com/problems/design-tinyurl](https://leetcode.com/problems/design-tinyurl) and it returns a short URL such as [http://tinyurl.com/4e9iAk](http://tinyurl.com/4e9iAk).

Design the encode and decode methods for the TinyURL service. There is no restriction on how your encode/decode algorithm should work. You just need to ensure that a URL can be encoded to a tiny URL and the tiny URL can be decoded to the original URL.

__题目描述__:
TinyURL是一种URL简化服务， 比如：当你输入一个URL [https://leetcode.com/problems/design-tinyurl](https://leetcode.com/problems/design-tinyurl) 时，它将返回一个简化的URL [http://tinyurl.com/4e9iAk](http://tinyurl.com/4e9iAk).

要求：设计一个 TinyURL 的加密 encode 和解密 decode 的方法。你的加密和解密算法如何设计和运作是没有限制的，你只需要保证一个URL可以被加密成一个TinyURL，并且这个TinyURL可以用解密方法恢复成原本的URL。

__思路__:

随机数➕ 哈希表
每次生成一个固定长度的字符串
如果哈希表中已经存在, 重新生成
时间复杂度 O(1), 空间复杂度 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    int count = 0;
    map<int, string> m;
public:
    // Encodes a URL to a shortened URL.
    string encode(string longUrl) 
    {
        m[count] = longUrl;
        return "http://" + to_string(count++);
    }

    // Decodes a shortened URL to its original URL.
    string decode(string shortUrl) 
    {
        return m[stol(shortUrl.substr(7, shortUrl.length() - 7))];
    }
};

// Your Solution object will be instantiated and called as such:
// Solution solution;
// solution.decode(solution.encode(url));
```

__Java__:

```Java
public class Codec {
    private String alphabet = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    private HashMap<String, String> map = new HashMap<>();
    private Random random = new Random();
    private String key = getRand();
    private final static int SIZE = 6;
    private final static String PREFIX = "http://tinyurl.com/";

    public String getRand() {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < SIZE; i++) sb.append(alphabet.charAt(random.nextInt(62)));
        return sb.toString();
    }

    public String encode(String longUrl) {
        while (map.containsKey(key)) key = getRand();
        map.put(key, longUrl);
        return PREFIX + key;
    }

    public String decode(String shortUrl) {
        return map.get(shortUrl.replace(PREFIX, ""));
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.decode(codec.encode(url));
```

__Python__:

```Python
class Codec:

    def encode(self, longUrl: str) -> str:
        """Encodes a URL to a shortened URL.
        """
        return longUrl
        

    def decode(self, shortUrl: str) -> str:
        """Decodes a shortened URL to its original URL.
        """
        return shortUrl
        

# Your Codec object will be instantiated and called as such:
# codec = Codec()
# codec.decode(codec.encode(url))
```
