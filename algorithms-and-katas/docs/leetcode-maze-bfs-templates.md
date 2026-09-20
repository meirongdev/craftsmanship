# 高效刷 LeetCode：用「基础模板 + 最小改动」记忆变种

> 一个小例子：如何高效地刷算法、刷 LeetCode。核心思想——基于基础题维持一个模板，**只记需要修改的部分**，把一个模板串起多道题，降低记忆负担，从而记住更多题目。

## 方法：基础模板 + 只记改动

- 选一道最基础的同族题当「模板」，把它练到能 2 分钟默写。
- 刷变种题时，**维持原模板不变**，只在需要变的地方标注【修改】。
- 记忆负担被压缩到「差异点」上，一个模板就能带起一串题。

## 第 0 步：基础模板——LeetCode 490 The Maze（BFS）

很基础的 BFS 题，相信大家都刷过。**如果不能 2 分钟写下来，先回去把它练熟。**

```python
from collections import deque

class Solution:
    def hasPath(self, maze: list[list[int]], start: list[int], destination: list[int]) -> bool:
        m, n = len(maze), len(maze[0])
        start_pt = (start[0], start[1])
        dst_pt = (destination[0], destination[1])

        visited = {start_pt}
        queue = deque([start_pt])  # 修正：dequeue -> deque

        dir = [(-1, 0), (1, 0), (0, 1), (0, -1)]

        while queue:
            r, c = queue.popleft()
            if (r, c) == dst_pt:
                return True  # 修正：retrurn -> return

            for dr, dc in dir:
                nr, nc = r, c
                # 修正：nc + dr -> nc + dc
                while 0 <= nr + dr < m and 0 <= nc + dc < n and maze[nr + dr][nc + dc] == 0:
                    nr += dr
                    nc += dc

                if (nr, nc) not in visited:
                    queue.append((nr, nc))
                    visited.add((nr, nc))

        return False
```

下面刷 **499 The Maze II**，要求找最短路径。怎么有效刷变种？怎么把信息压缩进记忆空间？答案：基于基础题，维持原模板，只记需要修改的部分。**关注【修改】的部分！！！**

## 第 1 步：499 The Maze II（Dijkstra，最短路径）

```python
import heapq  # 【修改 1】：引入 heapq 模块代替 collections.deque

class Solution:
    def shortestDistance(self, maze: list[list[int]], start: list[int], destination: list[int]) -> int:
        m, n = len(maze), len(maze[0])
        start_pt = (start[0], start[1])
        dst_pt = (destination[0], destination[1])

        # 【修改 2】：使用哈希表或二维数组记录到每个停靠点的最短距离
        # 初始时，起点的距离为 0
        dist_map = {start_pt: 0}

        # 【修改 3】：使用优先队列 (Min-Heap)，元素格式为 (d, r, c)，表示到达 (r, c) 的当前步数为 d
        heap = [(0, start_pt[0], start_pt[1])]

        directions = [(-1, 0), (1, 0), (0, 1), (0, -1)]

        while heap:
            # 【修改 4】：弹出当前全局步数最短的节点
            d, r, c = heapq.heappop(heap)

            # 【修改 5】：从堆顶弹出节点如果是终点，由于 Dijkstra 的性质，此时 d 必为全局最短步数
            if (r, c) == dst_pt:
                return d

            # 剪枝：如果弹出的步数大于已记录的最短步数，说明已被更优路径覆盖，直接跳过
            if d > dist_map.get((r, c), float('inf')):
                continue

            for dr, dc in directions:
                nr, nc = r, c
                step_count = 0  # 【修改 6】：新增变量，记录沿着当前方向滚动的格子数

                while 0 <= nr + dr < m and 0 <= nc + dc < n and maze[nr + dr][nc + dc] == 0:
                    nr += dr
                    nc += dc
                    step_count += 1  # 每向前一步，步数 +1

                # 新停靠点的总步数 = 当前步数 d + 这次滚动的步数 step_count
                new_dist = d + step_count

                # 【修改 7】：松弛条件——如果找到了到达 (nr, nc) 的更短路径，更新并压入优先队列
                if new_dist < dist_map.get((nr, nc), float('inf')):
                    dist_map[(nr, nc)] = new_dist
                    heapq.heappush(heap, (new_dist, nr, nc))

        # 【修改 8】：如果队列清空仍未到达终点，说明无法停在目标位置
        return -1
```

## 第 2 步：505 The Maze III（带洞，最短 + 字典序路径）

从 Maze II 改到 Maze III，核心有 3 个改动：

- **中途掉洞（Hole Check）**：在内部 while 循环里，每滚一步就要检查是否掉进洞里。如果到达洞口，立刻中断滚动！
- **状态记录增加路径字符串**：堆和最短距离哈希表中，除了记录步数 dist，还要同步记录路径 path_str（例如 "lul"）。
- **优先队列双重排序**：堆中元素存为 (dist, path_str, r, c)。Python 比较 Tuple 时，当 dist 相同时，会自动按照 path_str 的字典序排序，天然满足题目要求。

```python
import heapq

class Solution:
    def findShortestWay(self, maze: list[list[int]], ball: list[int], hole: list[int]) -> str:
        m, n = len(maze), len(maze[0])
        start_pt = (ball[0], ball[1])
        hole_pt = (hole[0], hole[1])

        # 【改动 1】：dist_map 存储 (r, c) -> (min_dist, min_path_str)
        # 初始时，起点步数为 0，路径字符串为空 ""
        dist_map = {start_pt: (0, "")}

        # 【改动 2】：堆元素扩展为 (dist, path_str, r, c)
        # Python 会先比较 dist，若 dist 相同则比较字典序最小的 path_str
        heap = [(0, "", start_pt[0], start_pt[1])]

        # 【改动 3】：方向列表附带字符 ('d', 'l', 'r', 'u')，且按字典序排列方便微观优选
        directions = [
            (1, 0, 'd'),
            (0, -1, 'l'),
            (0, 1, 'r'),
            (-1, 0, 'u')
        ]

        while heap:
            # 【改动 4】：弹出当前 (步数最短 > 字典序最小) 的节点
            d, path, r, c = heapq.heappop(heap)

            # 【改动 5】：弹出节点即为洞口时，直接返回路径字符串
            if (r, c) == hole_pt:
                return path

            # 剪枝：如果当前路径劣于记录的最优解（步数更大，或步数相同但字典序更大），跳过
            if (d, path) > dist_map.get((r, c), (float('inf'), "")):
                continue

            for dr, dc, char in directions:
                nr, nc = r, c
                step_count = 0

                while 0 <= nr + dr < m and 0 <= nc + dc < n and maze[nr + dr][nc + dc] == 0:
                    nr += dr
                    nc += dc
                    step_count += 1

                    # 【改动 6】：核心规则——如果滚动过程中踩到了洞口，立刻停下并进洞！
                    if (nr, nc) == hole_pt:
                        break

                new_dist = d + step_count
                new_path = path + char

                # 【改动 7】：松弛条件——如果找到了更短步数，或者步数相同但字典序更小的路径，则更新
                if (new_dist, new_path) < dist_map.get((nr, nc), (float('inf'), "")):
                    dist_map[(nr, nc)] = (new_dist, new_path)
                    heapq.heappush(heap, (new_dist, new_path, nr, nc))

        # 【改动 8】：若无法到洞里，题目要求返回 "impossible"
        return "impossible"
```

## 小结：一个模板带起 3 道题

| 题号 | 题型 | 相对上一题的改动 |
| --- | --- | --- |
| 490 The Maze | BFS 判可达 | 基础模板（deque + visited） |
| 499 The Maze II | 最短步数 | deque→heapq、visited→dist_map、加 step_count 与松弛 |
| 505 The Maze III | 最短 + 字典序 | 掉洞检查、状态加 path_str、堆按 (dist, path) 双键排序 |

这样，一个模板就可以带起 3 道题，脑子里理解记忆的负担也打折，可以记住更多的题目。按这个思路去试试吧，朋友们。
