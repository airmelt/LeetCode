# 2512 Reward Top K Students 奖励最顶尖的 K 名学生

__Description:__

You are given two string arrays `positive_feedback` and `negative_feedback`, containing the words denoting positive and negative feedback, respectively. Note that __no__ word is both positive and negative.

Initially every student has `0` points. Each positive word in a feedback report __increases__ the points of a student by `3`, whereas each negative word __decreases__ the points by `1`.

You are given `n` feedback reports, represented by a __0-indexed__ string array `report` and a __0-indexed__ integer array `student_id`, where `student_id[i]` represents the ID of the student who has received the feedback report `report[i]`. The ID of each student is __unique__.

Given an integer `k`, return _the top_ `k` _students after ranking them in __non-increasing__ order by their points_. In case more than one student has the same points, the one with the lower ID ranks higher.

__Example:__

Example 1:

```text
Input: positive_feedback = ["smart","brilliant","studious"], negative_feedback = ["not"], report = ["this student is studious","the student is smart"], student_id = [1,2], k = 2
Output: [1,2]
Explanation: 
Both the students have 1 positive feedback and 3 points but since student 1 has a lower ID he ranks higher.
```

Example 2:

```text
Input: positive_feedback = ["smart","brilliant","studious"], negative_feedback = ["not"], report = ["this student is not studious","the student is smart"], student_id = [1,2], k = 2
Output: [2,1]
Explanation: 
```

- The student with ID 1 has 1 positive feedback and 1 negative feedback, so he has 3-1=2 points.
- The student with ID 2 has 1 positive feedback, so he has 3 points.

Since student 2 has more points, [2,1] is returned.

__Constraints:__

- `1 <= positive_feedback.length, negative_feedback.length <= 10 ^ 4`
- `1 <= positive_feedback[i].length, negative_feedback[j].length <= 100`
- Both `positive_feedback[i]` and `negative_feedback[j]` consists of lowercase English letters.
- No word is present in both `positive_feedback` and `negative_feedback`.
- `n == report.length == student_id.length`
- `1 <= n <= 10 ^ 4`
- `report[i]` consists of lowercase English letters and spaces `' '`.
- There is a single space between consecutive words of `report[i]`.
- `1 <= report[i].length <= 100`
- `1 <= student_id[i] <= 10 ^ 9`
- All the values of `student_id[i]` are __unique__.
- `1 <= k <= n`

__题目描述:__

给你两个字符串数组 `positive_feedback` 和 `negative_feedback` ，分别包含表示正面的和负面的词汇。__不会__ 有单词同时是正面的和负面的。

一开始，每位学生分数为 `0` 。每个正面的单词会给学生的分数 __加__ `3` 分，每个负面的词会给学生的分数 __减__  `1` 分。

给你 `n` 个学生的评语，用一个下标从 __0__ 开始的字符串数组 `report` 和一个下标从 __0__ 开始的整数数组 `student_id` 表示，其中 `student_id[i]` 表示这名学生的 ID ，这名学生的评语是 `report[i]` 。每名学生的 ID __互不相同__。

给你一个整数 `k` ，请你返回按照得分 __从高到低__ 最顶尖的 `k` 名学生。如果有多名学生分数相同，ID 越小排名越前。

__示例:__

示例 1：

```text
输入：positive_feedback = ["smart","brilliant","studious"], negative_feedback = ["not"], report = ["this student is studious","the student is smart"], student_id = [1,2], k = 2
输出：[1,2]
解释：
两名学生都有 1 个正面词汇，都得到 3 分，学生 1 的 ID 更小所以排名更前。
```

示例 2：

```text
输入：positive_feedback = ["smart","brilliant","studious"], negative_feedback = ["not"], report = ["this student is not studious","the student is smart"], student_id = [1,2], k = 2
输出：[2,1]
解释：
```

- ID 为 1 的学生有 1 个正面词汇和 1 个负面词汇，所以得分为 3-1=2 分。
- ID 为 2 的学生有 1 个正面词汇，得分为 3 分。

学生 2 分数更高，所以返回 [2,1] 。

__提示：__

- `1 <= positive_feedback.length, negative_feedback.length <= 10 ^ 4`
- `1 <= positive_feedback[i].length, negative_feedback[j].length <= 100`
- `positive_feedback[i]` 和 `negative_feedback[j]` 都只包含小写英文字母。
- `positive_feedback` 和 `negative_feedback` 中不会有相同单词。
- `n == report.length == student_id.length`
- `1 <= n <= 10 ^ 4`
- `report[i]` 只包含小写英文字母和空格 `' '` 。
- `report[i]` 中连续单词之间有单个空格隔开。
- `1 <= report[i].length <= 100`
- `1 <= student_id[i] <= 10 ^ 9`
- `student_id[i]` 的值 __互不相同__ 。
- `1 <= k <= n`

__思路:__

```text
哈希表
将正面词汇的分数设为 3，负面词汇的分数设为 -1
遍历 report，统计每个学生的分数
使用优先队列存储学生的分数和 ID
或者使用排序
取前 k 名学生的 ID
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> topStudents(vector<string>& positive_feedback, vector<string>& negative_feedback, vector<string>& report, vector<int>& student_id, int k) 
    {
        unordered_map<string, int> score;
        for (const auto& word : positive_feedback) score[word] = 3;
        for (const auto& word : negative_feedback) --score[word];
        vector<vector<int>> heap;
        for (int i = 0, n = report.size(); i < n; i++) 
        {
            stringstream ss;
            string word;
            int cur = 0;
            ss << report[i];
            while (ss >> word) cur += score[word];
            heap.emplace_back(vector{-cur, student_id[i]});
        }
        sort(heap.begin(), heap.end());
        vector<int> result;
        for (int i = 0; i < k; i++) result.emplace_back(heap[i].back());
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> topStudents(String[] positive_feedback, String[] negative_feedback, String[] report, int[] student_id, int k) {
        var queue = new PriorityQueue<int[]>((a, b) -> a[1] == b[1] ? a[0] - b[0] : b[1] - a[1]);
        Map<String, Integer> score = new HashMap<>();
        for (var s : positive_feedback) score.put(s, 3);
        for (var s : negative_feedback) score.put(s, -1);
        for (int i = 0, n = report.length; i < n; i++) {
            String[] words = report[i].split(" ");
            int cur = 0;
            for (String word : words) cur += score.getOrDefault(word, 0);
            queue.offer(new int[]{student_id[i], cur});
        }
        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < k; i++) result.add(queue.poll()[0]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def topStudents(self, positive_feedback: List[str], negative_feedback: List[str], report: List[str], student_id: List[int], k: int) -> List[int]:
        return [e[1] for e in sorted(enumerate(student_id), key=lambda x: (sum(3 if e in positive_feedback else -1 if e in negative_feedback else 0 for e in report[x[0]].split()), -x[1]), reverse=True)[:k]] if (positive_feedback := set(positive_feedback)) and (negative_feedback := set(negative_feedback)) else sorted(student_id)[:k]
```
