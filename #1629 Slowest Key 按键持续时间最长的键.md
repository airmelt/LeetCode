# 1629 Slowest Key 按键持续时间最长的键

__Description:__

A newly designed keypad was tested, where a tester pressed a sequence of `n` keys, one at a time.

You are given a string `keysPressed` of length `n`, where `keysPressed[i]` was the `i ^ th` key pressed in the testing sequence, and a sorted list `releaseTimes`, where `releaseTimes[i]` was the time the `i ^ th` key was released. Both arrays are __0-indexed__. The `0 ^ th` key was pressed at the time `0`, and every subsequent key was pressed at the __exact__ time the previous key was released.

The tester wants to know the key of the keypress that had the __longest duration__. The `i ^ th` ^ keypress had a __duration__ of `releaseTimes[i] - releaseTimes[i - 1]`, and the `0 ^ th` keypress had a duration of `releaseTimes[0]`.

Note that the same key could have been pressed multiple times during the test, and these multiple presses of the same key may not have had the same duration.

Return the key of the keypress that had the longest duration. If there are multiple such keypresses, return the lexicographically largest key of the keypresses.

__Example:__

Example 1:

```text
Input: releaseTimes = [9,29,49,50], keysPressed = "cbcd"
Output: "c"
Explanation: The keypresses were as follows:
Keypress for 'c' had a duration of 9 (pressed at time 0 and released at time 9).
Keypress for 'b' had a duration of 29 - 9 = 20 (pressed at time 9 right after the release of the previous character and released at time 29).
Keypress for 'c' had a duration of 49 - 29 = 20 (pressed at time 29 right after the release of the previous character and released at time 49).
Keypress for 'd' had a duration of 50 - 49 = 1 (pressed at time 49 right after the release of the previous character and released at time 50).
The longest of these was the keypress for 'b' and the second keypress for 'c', both with duration 20.
'c' is lexicographically larger than 'b', so the answer is 'c'.
```

Example 2:

```text
Input: releaseTimes = [12,23,36,46,62], keysPressed = "spuda"
Output: "a"
Explanation: The keypresses were as follows:
Keypress for 's' had a duration of 12.
Keypress for 'p' had a duration of 23 - 12 = 11.
Keypress for 'u' had a duration of 36 - 23 = 13.
Keypress for 'd' had a duration of 46 - 36 = 10.
Keypress for 'a' had a duration of 62 - 46 = 16.
The longest of these was the keypress for 'a' with duration 16.
```

__Constraints:__

- `releaseTimes.length == n`
- `keysPressed.length == n`
- `2 <= n <= 1000`
- `1 <= releaseTimes[i] <= 10 ^ 9`
- `releaseTimes[i] < releaseTimes[i+1]`
- `keysPressed` contains only lowercase English letters.

__题目描述:__

A newly designed keypad was tested, where a tester pressed a sequence of `n` keys, one at a time.

You are given a string `keysPressed` of length `n`, where `keysPressed[i]` was the `i ^ th` key pressed in the testing sequence, and a sorted list `releaseTimes`, where `releaseTimes[i]` was the time the `i ^ th` key was released. Both arrays are __0-indexed__. The `0 ^ th` key was pressed at the time `0`, and every subsequent key was pressed at the __exact__ time the previous key was released.

The tester wants to know the key of the keypress that had the __longest duration__. The `i ^ th` ^ keypress had a __duration__ of `releaseTimes[i] - releaseTimes[i - 1]`, and the `0 ^ th` keypress had a duration of `releaseTimes[0]`.

Note that the same key could have been pressed multiple times during the test, and these multiple presses of the same key may not have had the same duration.

Return the key of the keypress that had the longest duration. If there are multiple such keypresses, return the lexicographically largest key of the keypresses.

__Example:__

Example 1:

```text
Input: releaseTimes = [9,29,49,50], keysPressed = "cbcd"
Output: "c"
Explanation: The keypresses were as follows:
Keypress for 'c' had a duration of 9 (pressed at time 0 and released at time 9).
Keypress for 'b' had a duration of 29 - 9 = 20 (pressed at time 9 right after the release of the previous character and released at time 29).
Keypress for 'c' had a duration of 49 - 29 = 20 (pressed at time 29 right after the release of the previous character and released at time 49).
Keypress for 'd' had a duration of 50 - 49 = 1 (pressed at time 49 right after the release of the previous character and released at time 50).
The longest of these was the keypress for 'b' and the second keypress for 'c', both with duration 20.
'c' is lexicographically larger than 'b', so the answer is 'c'.
```

Example 2:

```text
Input: releaseTimes = [12,23,36,46,62], keysPressed = "spuda"
Output: "a"
Explanation: The keypresses were as follows:
Keypress for 's' had a duration of 12.
Keypress for 'p' had a duration of 23 - 12 = 11.
Keypress for 'u' had a duration of 36 - 23 = 13.
Keypress for 'd' had a duration of 46 - 36 = 10.
Keypress for 'a' had a duration of 62 - 46 = 16.
The longest of these was the keypress for 'a' with duration 16.
```

__Constraints:__

- `releaseTimes.length == n`
- `keysPressed.length == n`
- `2 <= n <= 1000`
- `1 <= releaseTimes[i] <= 10 ^ 9`
- `releaseTimes[i] < releaseTimes[i+1]`
- `keysPressed` contains only lowercase English letters.

__思路:__

```text
模拟
需要判断持续最长时间的按键
当持续时间相同时再判断字符顺序
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    char slowestKey(vector<int>& releaseTimes, string keysPressed) 
    {
        int n = releaseTimes.size(), m = releaseTimes.front(), cur = 0, c = keysPressed.front();
        for (int i = 1; i < n; i++) 
        {
            if ((cur = releaseTimes[i] - releaseTimes[i - 1]) > m) 
            {
                m = cur;
                c = keysPressed[i];
            } else if (cur == m and keysPressed[i] > c) c = keysPressed[i];
        }
        return c;
    }
};
```

__Java__:

```Java
class Solution {
    public char slowestKey(int[] releaseTimes, String keysPressed) {
        int n = releaseTimes.length, m = releaseTimes[0], cur = 0, c = keysPressed.charAt(0);
        for (int i = 1; i < n; i++) {
            if ((cur = releaseTimes[i] - releaseTimes[i - 1]) > m) {
                m = cur;
                c = keysPressed.charAt(i);
            } else if (cur == m && keysPressed.charAt(i) > c) c = keysPressed.charAt(i);
        }
        return (char)c;
    }
}
```

__Python__:

```Python
class Solution:
    def slowestKey(self, releaseTimes: List[int], keysPressed: str) -> str:
        return max(zip(map(int.__sub__, releaseTimes, [0] + releaseTimes), keysPressed))[1]
```
