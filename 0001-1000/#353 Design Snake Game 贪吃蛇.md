# 353 Design Snake Game 贪吃蛇

__Description:__

Design a [Snake game](https://en.wikipedia.org/wiki/Snake_(disambiguation)#Games_and_toys) that is played on a device with screen size `height x width`. [Play the game online](https://patorjk.com/games/snake/) if you are not familiar with the game.

The snake is initially positioned at the top left corner `(0, 0)` with a length of `1` unit.

You are given an array food where `food[i] = (ri, ci)` is the row and column position of a piece of food that the snake can eat. When a snake eats a piece of food, its length and the game's score both increase by `1`.

Each piece of food appears one by one on the screen, meaning the second piece of food will not appear until the snake eats the first piece of food.

When a piece of food appears on the screen, it is __guaranteed__ that it will not appear on a block occupied by the snake.

The game is over if the snake goes out of bounds (hits a wall) or if its head occupies a space that its body occupies __after__ moving (i.e. a snake of length 4 cannot run into itself).

Implement the `SnakeGame` class:

- `SnakeGame(int width, int height, int[][] food)` Initializes the object with a screen of size `height x width` and the positions of the `food`.
- `int move(String direction)` Returns the score of the game after applying one `direction` move by the snake. If the game is over, return `-1`.

__Example:__

Example 1:

![353-1](https://assets.leetcode.com/uploads/2021/01/13/snake.jpg)

```text
Input
["SnakeGame", "move", "move", "move", "move", "move", "move"]
[[3, 2, [[1, 2], [0, 1]]], ["R"], ["D"], ["R"], ["U"], ["L"], ["U"]]
Output
[null, 0, 0, 1, 1, 2, -1]

Explanation
```

```Java
SnakeGame snakeGame = new SnakeGame(3, 2, [[1, 2], [0, 1]]);
snakeGame.move("R"); // return 0
snakeGame.move("D"); // return 0
snakeGame.move("R"); // return 1, snake eats the first piece of food. The second piece of food appears at (0, 1).
snakeGame.move("U"); // return 1
snakeGame.move("L"); // return 2, snake eats the second food. No more food appears.
snakeGame.move("U"); // return -1, game over because snake collides with border
```

__Constraints:__

- `1 <= width, height <= 10 ^ 4`
- `1 <= food.length <= 50`
- `food[i].length == 2`
- `0 <= ri < height`
- `0 <= ci < width`
- `direction.length == 1`
- `direction` is `'U'`, `'D'`, `'L'`, or `'R'`.
- At most `10 ^ 4` calls will be made to `move`.

__题目描述:__

请你设计一个 [贪吃蛇游戏](https://baike.baidu.com/item/%E8%B4%AA%E5%90%83%E8%9B%87/9510203?fr=aladdin)，该游戏将会在一个 __屏幕尺寸 = 宽度 x 高度__ 的屏幕上运行。如果你不熟悉这个游戏，可以 [点击这里](https://patorjk.com/games/snake/) 在线试玩。

起初时，蛇在左上角的 `(0, 0)` 位置，身体长度为 `1` 个单位。

你将会被给出一个数组形式的食物位置序列 `food` ，其中 `food[i] = (ri, ci)` 。当蛇吃到食物时，身子的长度会增加 `1` 个单位，得分也会 `+1` 。

食物不会同时出现，会按列表的顺序逐一显示在屏幕上。比方讲，第一个食物被蛇吃掉后，第二个食物才会出现。

当一个食物在屏幕上出现时，保证 不会 出现在被蛇身体占据的格子里。

如果蛇越界（与边界相撞）或者头与 移动后 的身体相撞（即，身长为 `4` 的蛇无法与自己相撞），游戏结束。

实现 `SnakeGame` 类：

- `SnakeGame(int width, int height, int[][] food)` 初始化对象，屏幕大小为 `height x width` ，食物位置序列为 `food`
- `int move(String direction)` 返回蛇在方向 `direction` 上移动后的得分。如果游戏结束，返回 `-1` 。

__示例:__

![353-2](https://assets.leetcode.com/uploads/2021/01/13/snake.jpg)

示例 1：

```text
输入：
["SnakeGame", "move", "move", "move", "move", "move", "move"]
[[3, 2, [[1, 2], [0, 1]]], ["R"], ["D"], ["R"], ["U"], ["L"], ["U"]]
输出：
[null, 0, 0, 1, 1, 2, -1]

解释：
```

```Java
SnakeGame snakeGame = new SnakeGame(3, 2, [[1, 2], [0, 1]]);
snakeGame.move("R"); // 返回 0
snakeGame.move("D"); // 返回 0
snakeGame.move("R"); // 返回 1 ，蛇吃掉了第一个食物，同时第二个食物出现在 (0, 1)
snakeGame.move("U"); // 返回 1
snakeGame.move("L"); // 返回 2 ，蛇吃掉了第二个食物，没有出现更多食物
snakeGame.move("U"); // 返回 -1 ，蛇与边界相撞，游戏结束
```

__提示：__

- `1 <= width, height <= 10 ^ 4`
- `1 <= food.length <= 50`
- `food[i].length == 2`
- `0 <= ri < height`
- `0 <= ci < width`
- `direction.length == 1`
- `direction` 是 `'U'`, `'D'`, `'L'`, 或 `'R'`.
- 最多调用 `10 ^ 4` 次 `move` 方法.

__思路:__

```text
模拟
将二维坐标转换为一维坐标, cur = x * width + y
用一个哈希表存储蛇走过的路径
用一个双端队列存储蛇的身体
先判断蛇是否越界
然后尝试吃食物, 如果吃到食物, 则食物坐标后移并得分
如果没有吃到食物, 则蛇尾出队
如果蛇头和蛇身相撞, 则游戏结束
否则蛇头入队
```

__代码:__

__C++__:

```C++
class SnakeGame 
{
public:
    SnakeGame(int width, int height, vector<vector<int>>& food): width(width), height(height), food(food) {}
    
    int move(string direction) 
    {
        int head = snake.front(), x = head / width - (direction == "U") + (direction == "D"), y = head % width - (direction == "L") + (direction == "R"), cur = x * width + y;
        if (!(-1 < x and x < height and -1 < y and y < width)) return GAME_OVER;
        if (food_index < food.size() and x == food[food_index].front() and y == food[food_index].back()) 
        {
            ++food_index;
            ++score;
        }
        else
        {
            visited.erase(snake.back());
            snake.pop_back();
        }
        if (!visited.count(cur)) 
        {
            visited.insert(cur);
            snake.emplace_front(cur);
            return score;
        }
        return GAME_OVER;
    }
private:
    int width, height, food_index = 0, score = 0, GAME_OVER = -1;
    vector<vector<int>> food;
    deque<int> snake{0};
    unordered_set<int> visited{0};
};

/**
 * Your SnakeGame object will be instantiated and called as such:
 * SnakeGame* obj = new SnakeGame(width, height, food);
 * int param_1 = obj->move(direction);
 */
```

__Java__:

```Java
class SnakeGame {
    private int width, height, foodIndex = 0, score = 0, GAME_OVER = -1;
    private int[][] food;
    private Deque<Integer> snake = new ArrayDeque<>(){{ addLast(0); }};
    private Set<Integer> visited = new HashSet<>(){{ add(0); }};

    public SnakeGame(int width, int height, int[][] food) {
        this.width = width;
        this.height = height;
        this.food = food;
    }
    
    public int move(String direction) {
        int head = snake.peekFirst(), x = head / width - (direction.equals("U") ? 1 : 0) + (direction.equals("D") ? 1 : 0), y = head % width - (direction.equals("L") ? 1 : 0) + (direction.equals("R") ? 1 : 0), cur = x * width + y;
        if (!(-1 < x && x < height && -1 < y && y < width)) return GAME_OVER;
        if (foodIndex < food.length && x == food[foodIndex][0] && y == food[foodIndex][1]) {
            ++foodIndex;
            ++score;
        } else visited.remove(snake.pollLast());
        if (!visited.contains(cur)) {
            visited.add(cur);
            snake.addFirst(cur);
            return score;
        }
        return GAME_OVER;
    }
}

/**
 * Your SnakeGame object will be instantiated and called as such:
 * SnakeGame obj = new SnakeGame(width, height, food);
 * int param_1 = obj.move(direction);
 */
```

__Python__:

```Python
class SnakeGame:

    def __init__(self, width: int, height: int, food: List[List[int]]):
        self.head = [0, 0]
        self.snake = deque([self.head])
        self.food = deque(food)
        self.shape = (height, width)
        self.score = 0
        

    def move(self, direction: str) -> int:
        if not -1 < (x := self.head[0] - (direction == 'U') + (direction == 'D')) < self.shape[0] or not -1 < (y := self.head[1] - (direction == 'L') + (direction == 'R')) < self.shape[1]:
            return -1
        self.head = [x, y]
        self.snake.appendleft(self.head)
        if self.food and self.head == self.food[0]:
            self.score += 1
            self.food.popleft()
        else:
            self.snake.pop()
        return self.score if not any(self.head == body for body in list(self.snake)[1:]) else -1
        


# Your SnakeGame object will be instantiated and called as such:
# obj = SnakeGame(width, height, food)
# param_1 = obj.move(direction)
```
