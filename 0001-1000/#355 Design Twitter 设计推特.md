# 355 Design Twitter 设计推特

__Description__:
Design a simplified version of Twitter where users can post tweets, follow/unfollow another user and is able to see the 10 most recent tweets in the user's news feed. Your design should support the following methods:

postTweet(userId, tweetId): Compose a new tweet.
getNewsFeed(userId): Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent.
follow(followerId, followeeId): Follower follows a followee.
unfollow(followerId, followeeId): Follower unfollows a followee.

__Example:__

```Java
Twitter twitter = new Twitter();

// User 1 posts a new tweet (id = 5).
twitter.postTweet(1, 5);

// User 1's news feed should return a list with 1 tweet id -> [5].
twitter.getNewsFeed(1);

// User 1 follows user 2.
twitter.follow(1, 2);

// User 2 posts a new tweet (id = 6).
twitter.postTweet(2, 6);

// User 1's news feed should return a list with 2 tweet ids -> [6, 5].
// Tweet id 6 should precede tweet id 5 because it is posted after tweet id 5.
twitter.getNewsFeed(1);

// User 1 unfollows user 2.
twitter.unfollow(1, 2);

// User 1's news feed should return a list with 1 tweet id -> [5],
// since user 1 is no longer following user 2.
twitter.getNewsFeed(1);
```

__题目描述__:
设计一个简化版的推特(Twitter)，可以让用户实现发送推文，关注/取消关注其他用户，能够看见关注人（包括自己）的最近十条推文。你的设计需要支持以下的几个功能：

postTweet(userId, tweetId): 创建一条新的推文
getNewsFeed(userId): 检索最近的十条推文。每个推文都必须是由此用户关注的人或者是用户自己发出的。推文必须按照时间顺序由最近的开始排序。
follow(followerId, followeeId): 关注一个用户
unfollow(followerId, followeeId): 取消关注一个用户

__示例 :__

```Java
Twitter twitter = new Twitter();

// 用户1发送了一条新推文 (用户id = 1, 推文id = 5).
twitter.postTweet(1, 5);

// 用户1的获取推文应当返回一个列表，其中包含一个id为5的推文.
twitter.getNewsFeed(1);

// 用户1关注了用户2.
twitter.follow(1, 2);

// 用户2发送了一个新推文 (推文id = 6).
twitter.postTweet(2, 6);

// 用户1的获取推文应当返回一个列表，其中包含两个推文，id分别为 -> [6, 5].
// 推文id6应当在推文id5之前，因为它是在5之后发送的.
twitter.getNewsFeed(1);

// 用户1取消关注了用户2.
twitter.unfollow(1, 2);

// 用户1的获取推文应当返回一个列表，其中包含一个id为5的推文.
// 因为用户1已经不再关注用户2.
twitter.getNewsFeed(1);
```

__思路__:

使用 3个表
一个表用来记录关注的人及发布的推特
一个表用来记录发表的推特及时间
一个表用来记录用户的 id和他的关注
另外一个全局变量用来记录最多发布的推特, 10条, 超过 10条的需要删除队首, 因为不用显示
一个全局变量当作时间戳, 记录推特发布的时间
发布推特时, 如果发布的推特已经达到上限, 弹出最先发布的, 保证链表队列中只有不多于 10条推特
需要判断是否自己关注自己
获取推特时间复杂度O(n), 发推特, 关注, 取关时间复杂度O(1), 空间复杂度O(n), n表示关注的数量

__代码__:
__C++__:

```C++
class Twitter 
{
private:
    void init(int userId) 
    {
        user[userId].followee.clear();
        user[userId].tweet.clear();
    }
    
    struct Node 
    {
        unordered_set<int> followee;
        list<int> tweet;
    };
    int recent_max, time;
    unordered_map<int, int> tweet_time;
    unordered_map<int, Node> user;
public:
    /** Initialize your data structure here. */
    Twitter() 
    {
        time = 0;
        recent_max = 10;
        user.clear();
    }
    
    /** Compose a new tweet. */
    void postTweet(int userId, int tweetId) 
    {
        if (user.find(userId) == user.end()) init(userId);
        if (user[userId].tweet.size() == recent_max) user[userId].tweet.pop_back();
        user[userId].tweet.push_front(tweetId);
        tweet_time[tweetId] = ++time;
    }
    
    /** Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent. */
    vector<int> getNewsFeed(int userId) 
    {
        vector<int> result; 
        result.clear();
        for (auto it = user[userId].tweet.begin(); it != user[userId].tweet.end(); ++it) result.emplace_back(*it);
        for (auto followeeId: user[userId].followee) 
        {
            if (followeeId == userId) continue;
            vector<int> cur; 
            cur.clear();
            auto it = user[followeeId].tweet.begin();
            int i = 0;
            while (i < result.size() and it != user[followeeId].tweet.end()) 
            {
                if (tweet_time[(*it)] > tweet_time[result[i]]) 
                {
                    cur.emplace_back(*it);
                    ++it;
                } 
                else 
                {
                    cur.emplace_back(result[i]);
                    ++i;
                }
                if (cur.size() == recent_max) break;
            }
            for (; i < result.size() and cur.size() < recent_max; ++i) cur.emplace_back(result[i]);
            for (; it != user[followeeId].tweet.end() && cur.size() < recent_max; ++it) cur.emplace_back(*it);
            result.assign(cur.begin(),cur.end());
        }
        return result;
    }
    
    /** Follower follows a followee. If the operation is invalid, it should be a no-op. */
    void follow(int followerId, int followeeId) 
    {
        if (user.find(followerId) == user.end()) init(followerId);
        if (user.find(followeeId) == user.end()) init(followeeId);
        user[followerId].followee.insert(followeeId);
    }
    
    /** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
    void unfollow(int followerId, int followeeId) 
    {
        user[followerId].followee.erase(followeeId);
    }
};

/**
 * Your Twitter object will be instantiated and called as such:
 * Twitter* obj = new Twitter();
 * obj->postTweet(userId,tweetId);
 * vector<int> param_2 = obj->getNewsFeed(userId);
 * obj->follow(followerId,followeeId);
 * obj->unfollow(followerId,followeeId);
 */
```

__Java__:

```Java
class Twitter {
    private class Node {
        Set<Integer> followee;
        LinkedList<Integer> tweet;

        Node() {
            followee = new HashSet<>();
            tweet = new LinkedList<>();
        }
    }

    private int recentMax, time;
    private Map<Integer, Integer> tweetTime;
    private Map<Integer, Node> user;

    /** Initialize your data structure here. */
    public Twitter() {
        time = 0;
        recentMax = 10;
        tweetTime = new HashMap<>();
        user = new HashMap<>();
    }

    public void init(int userId) {
        user.put(userId, new Node());
    }
    
    /** Compose a new tweet. */
    public void postTweet(int userId, int tweetId) {
        if (!user.containsKey(userId)) init(userId);
        if (user.get(userId).tweet.size() == recentMax) user.get(userId).tweet.remove(recentMax - 1);
        user.get(userId).tweet.addFirst(tweetId);
        tweetTime.put(tweetId, ++time);
    }
    
    /** Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent. */
    public List<Integer> getNewsFeed(int userId) {
        LinkedList<Integer> result = new LinkedList<Integer>();
        for (int it : user.getOrDefault(userId, new Node()).tweet) result.addLast(it);
        for (int followeeId : user.getOrDefault(userId, new Node()).followee) {
            if (followeeId == userId) continue;
            LinkedList<Integer> cur = new LinkedList<Integer>();
            int tweetSize = user.get(followeeId).tweet.size();
            Iterator<Integer> it = user.get(followeeId).tweet.iterator();
            int i = 0, j = 0, curr = -1;
            if (j < tweetSize) {
                curr = it.next();
                while (i < result.size() && j < tweetSize) {
                    if (tweetTime.get(curr) > tweetTime.get(result.get(i))) {
                        cur.addLast(curr);
                        ++j;
                        if (it.hasNext()) curr = it.next();
                    } else {
                        cur.addLast(result.get(i));
                        ++i;
                    }
                    if (cur.size() == recentMax) break;
                }
            }
            for (; i < result.size() && cur.size() < recentMax; ++i) cur.addLast(result.get(i));
            if (j < tweetSize && cur.size() < recentMax) {
                cur.addLast(curr);
                for (; it.hasNext() && cur.size() < recentMax;)  cur.addLast(it.next());
            }
            result = new LinkedList<Integer>(cur);
        }
        return result;
    }
    
    /** Follower follows a followee. If the operation is invalid, it should be a no-op. */
    public void follow(int followerId, int followeeId) {
        if (!user.containsKey(followerId)) init(followerId);
        if (!user.containsKey(followeeId)) init(followeeId);
        user.get(followerId).followee.add(followeeId);
    }
    
    /** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
    public void unfollow(int followerId, int followeeId) {
        user.getOrDefault(followerId, new Node()).followee.remove(followeeId);
    }
}

/**
 * Your Twitter object will be instantiated and called as such:
 * Twitter obj = new Twitter();
 * obj.postTweet(userId,tweetId);
 * List<Integer> param_2 = obj.getNewsFeed(userId);
 * obj.follow(followerId,followeeId);
 * obj.unfollow(followerId,followeeId);
 */
```

__Python__:

```Python
class Twitter:
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.twit_list = {}
        self.follow_list = {}
        self.time = 0
        self.recent_max = 10
        

    def postTweet(self, userId: int, tweetId: int) -> None:
        """
        Compose a new tweet.
        """
        self.time += 1
        if self.twit_list.get(userId) is None:
            self.twit_list[userId] = []
        if len(self.twit_list[userId]) == self.recent_max:
            self.twit_list[userId].pop(0)
        self.twit_list[userId].append([tweetId, self.time])
        

    def getNewsFeed(self, userId: int) -> List[int]:
        """
        Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent.
        """
        ids = self.follow_list.get(userId, set())
        ids.add(userId)
        tweets = []
        for id in ids:
            tweets += self.twit_list.get(id, [])
        tweets.sort(key=lambda x: x[1], reverse=True)
        result = []
        for tweet in tweets:
            result.append(tweet[0])
        return result[:10]

    
    def follow(self, followerId: int, followeeId: int) -> None:
        """
        Follower follows a followee. If the operation is invalid, it should be a no-op.
        """
        if self.follow_list.get(followerId) is None:
            self.follow_list[followerId] = set()
        self.follow_list[followerId].add(followeeId)

        
    def unfollow(self, followerId: int, followeeId: int) -> None:
        """
        Follower unfollows a followee. If the operation is invalid, it should be a no-op.
        """
        if self.follow_list.get(followerId) is None:
            return
        self.follow_list[followerId].discard(followeeId)


# Your Twitter object will be instantiated and called as such:
# obj = Twitter()
# obj.postTweet(userId,tweetId)
# param_2 = obj.getNewsFeed(userId)
# obj.follow(followerId,followeeId)
# obj.unfollow(followerId,followeeId)
```
