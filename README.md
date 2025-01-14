# Stack, Queue, Depue

## 1) Stack
- LIFO(Last-In-First-Out)
```python
List.append()
List.pop()
```
- [VPS (Valid Parenthesis String)](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/valid_parenthesis_string.py):
	- Time complexity: $O(n)$, Space complexity: $O(n)$
- [NGE (Next Greater Element)](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/next_greater_element.py):
	- Time complexity: $O(n)$, Space complexity: $O(n)$

## 2) Queue
- LILO(Last-In-Last-Out)
- 데이터가 들어오는 위치는 가장 뒤(Rear 또는 Back이라고 한다.)에 있고, 데이터가 나가는 위치는 가장 앞(Front라고 한다.)
- `List.pop(0)` 사용 시 첫 번째 element를 pop한 후 나머지 elements의 Index를 1칸씩 당기는 과정에서 O(n)의 계산량이 발생한다.(Source: https://www.acmicpc.net/board/view/47845)
```python
from collections import deque

deque().append()
deque().popleft()
```
### (1) Priority Queue
- Priority queue can be implemented by heap data structure.

## 3) Deque (Double Ended Queue)
```python
from collections import deque

deque().append()
deque().appendleft()
deque().pop()
deque().popleft()
```

# 2. Graph Traversal
- Source: https://en.wikipedia.org/wiki/Graph_theory
- Graph is made up of vertices (also called nodes or points) which are connected by edges (also called links or lines).
- Undirected graph is a graph where edges link two vertices symmetrically, and directed graph is a the one where edges link two vertices asymmetrically.
- Source: https://www.acmicpc.net/problem/15681
- 간선의 집합에 변함이 없는 한, 그래프는 얼마든지 원하는 대로 다시 그릴 수가 있다.
- Source: https://en.wikipedia.org/wiki/Cycle_(graph_theory)
- A cycle in a graph is a non-empty trail in which the only repeated vertices are the first and last vertices. A directed cycle in a directed graph is a non-empty directed trail in which the only repeated vertices are the first and last vertices.

## 1) DFS (Depth First Search)
```python
def dfs(start_node):
	stack = [start_node]
	while stack:
		cur_node = stack.pop()
		visited[cur_node] = True # Optional.
		### DO SOMETHING
		if visited[cur_node] == False: # Optional.
			visited[cur_node] = True # Optional.
			stack.extend(graph[cur_node])


if __name__ == "__main__":
	dfs(start_node)
...
```

### (1) Backtracking
- [Subset sum problem](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/subset_sum.py)
- [N-queens problem](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/n_queens.py)

## 2) Breadth First Search
```python
from collections import deque

while queue:
	cur_node = queue.popleft()
	if visited[cur_node] == False:
		visited[cur_node] = True
		queue.extend(graph[cur_node])
```
- 간선에 가중치가 없다면 Shortest path problem에 적용 가능합니다.
```python
from collections import deque

queue = deque([...])
visited = {...}
while queue:
	cur_node, cur_dist = queue.popleft()
	if visited[cur_node] == False:
		visited[cur_node] = True
		for end, dist in graph[cur_node]:
			queue.append((end, cur_dist + dist))
```

## 3) Dijkstra Algorithm
- Time complexity: O((V + E)logV)
- 각 간선에 가중치가 있다면 사용 가능합니다. 간선들 중 단 하나라도 가중치가 음수이면 이 알고리즘은 사용할 수 없습니다.
```python
import math

# 지금까지 알려진, 출발지에서 해당 노드로 가는 최단 거리를 기록하는 Dictionary를 만듭니다.
min_dists = {i:0 if i == start else math.inf for i in range(1, V + 1)}
hp = [(0, start)]
while hp:
    cur_dist, cur_node = heapq.heappop(hp)
    for dist, next_node in graph[cur_node]:
		# 지금까지 알려진 `next_node`까지의 최단 거리보다 `cur_node`를 거쳐서 `next_node`로 가는 거리가 더 짧다면 그 값으로 `min_dists[next_node]`를 업데이트합니다.
        if cur_dist + dist < min_dists[next_node]:
            min_dists[next_node] = cur_dist + dist
            heapq.heappush(hp, (min_dists[next_node], next_node))
```

## 4) Bellman–Ford Algorithm
- Time complexity: O(VE)
- Source: https://en.wikipedia.org/wiki/Bellman%E2%80%93Ford_algorithm
- The Bellman–Ford algorithm is an algorithm that computes shortest paths from a single source vertex to all of the other vertices in a weighted digraph.
- It is slower than Dijkstra's algorithm for the same problem, but more versatile, as it is capable of handling graphs in which some of the edge weights are negative numbers.
- If a graph contains a "negative cycle" (i.e. a cycle whose edges sum to a negative value) that is reachable from the source, then there is no cheapest path: any path that has a point on the negative cycle can be made cheaper by one more walk around the negative cycle. In such a case, the Bellman–Ford algorithm can detect and report the negative cycle.
- Source: https://velog.io/@younge/Python-%EC%B5%9C%EB%8B%A8-%EA%B2%BD%EB%A1%9C-%EB%B2%A8%EB%A7%8C-%ED%8F%AC%EB%93%9CBellman-Ford-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98
- 음수 사이클 존재의 여부를 알 수 있다.
- 음수 사이클 안에서 무한으로 뺑뺑이 도는 경우를 알 수 있는 방법은, 그래프 정점의 개수를 V라고 할 때 인접 간선을 검사하고 거리 값을 갱신하는 과정을 V-1번으로 제한하면 가능해진다. 그래프의 시작 정점에서 특정 정점까지 도달하기 위해 거쳐 가는 최대 간선 수는 V-1개이기 때문에 V번째 간선이 추가되는 순간 사이클이라고 판단할 수 있게 된다.
- 위 과정을 모두 마치고 난 후 거리가 갱신되는 경우가 생긴다면 그래프에 음수 사이클이 존재한다는 것이다.
- Source: https://deep-learning-study.tistory.com/587
- 다익스트라와의 차이점은 매 반복마다 모든 간선을 확인한다는 것입니다. 다익스트라는 방문하지 않는 노드 중에서 최단 거리가 가장 가까운 노드만을 방문했습니다.
```python
import math

min_dists = {i:0 if i == start else math.inf for i in range(1, V + 1)}
breaker1 = False
breaker2 = False
for i in range(V):
    for cur_node in range(1, V + 1):
        for dist, next_node in graph[cur_node]:
            # 지금까지 알려진 `next_node`로의 최단 거리보다 `cur_node`를 거쳐서 가는 경로가 더 짧다면 이 값으로 `min_dists[next_node]`를 업데이트합니다.
            if dist + min_dists[cur_node] < min_dists[next_node]:
                min_dists[next_node] = dist + min_dists[cur_node]
                # `V - 1`번 `min_dists`를 업데이트한 이후에도 또 업데이트가 된다면 Negative cycle이 존재하는 것입니다.
                if i == V - 1:
                    breaker1 = True
                    break
        if breaker1 == True:
            breaker2 = True
            break
    if breaker2 == True:
        print(-1)
        break
else:
    ...
```

## 5) Floyd–Warshall Algorithm
- Time complexity: O(V^3)
- Source: https://en.wikipedia.org/wiki/Floyd%E2%80%93Warshall_algorithm
- Floyd–Warshall algorithm is an algorithm for finding shortest paths in a directed weighted graph with positive or negative edge weights (but with no negative cycles).
- A single execution of the algorithm will find the lengths (summed weights) of shortest paths between all pairs of vertices. Although it does not return details of the paths themselves, it is possible to reconstruct the paths with simple modifications to the algorithm.
```python
import math
from itertools import product

min_dists = {(i, j):0 if i == j else math.inf for i, j in product(range(1, V + 1), range(1, V + 1))}

for btw in range(1, V + 1):
    for start in range(1, V + 1):
        for end in range(1, V + 1):
            if min_dists[(start, btw)] + min_dists[(btw, end)] < min_dists[(start, end)]:
                min_dists[(start, end)] = min_dists[(start, btw)] + min_dists[(btw, end)]
```

## 6) Tree
- Source: https://en.wikipedia.org/wiki/Cycle_(graph_theory)
- A graph without cycles is called an acyclic graph. A directed graph without directed cycles is called a Directed Acyclic Graph(DAG, tree).
- Source: https://www.acmicpc.net/problem/15681
- 임의의 두 정점 U와 V에 대해, U에서 V로 가는 최단경로는 유일하다.
아무 정점이나 잡고 부모와의 연결을 끊었을 때, 해당 정점과 그 자식들, 그 자식들의 자식들… 로 이루어진 부분그래프는 트리가 된다.
- There are (n - 1) edges in a tree with n nodes.
- Source: https://en.wikipedia.org/wiki/Tree_(data_structure)
- Ancestor: A node reachable by repeated proceeding from child to parent.
- Descendant, Subchild: A node reachable by repeated proceeding from parent to child.
- Degree: For a given node, its number of children. A leaf has necessarily degree zero.
	- Degree of a tree: Maximum degree of a node in the tree.
- Distance: The number of edges along the shortest path between two nodes.
- Level: The level of a node is the number of edges along the unique path between it and the root node.
- Width: The number of nodes in a level.
- Bredth: The number of leaves.
- Diameter of a tree: The longest path between any two nodes (which may or may not pass through the root node).
	```python
	# Choose a node arbitrarily.
	node = list(tree)[0]

	stack = [(node, 0)]
	visited = {i:False for i in tree}

	# 지름을 구성하는 2개의 노드 중 하나를 구합니다.
	max_dist = 1
	while stack:
		start, dist1 = stack.pop()
		if visited[start] == False:
			visited[start] = True
			if dist1 > max_dist:
				max_dist = dist1
				max_node = start
			for end, dist2 in tree[start]:
				stack.append((end, dist1 + dist2))

	stack = [(max_node, 0)]
	visited = {i:False for i in tree}
	visited[node] = True

	# Get the diameter.
	diam = 1
	while stack:
		start, dist1 = stack.pop()
		if visited[start] == False:
			visited[start] = True
			if dist1 > diam:
				diam = dist1
			for end, dist2 in tree[start]:
				stack.append((end, dist1 + dist2))
	```
- Subtree
	```python
	def subtree(cur_node) :
		size[cur_node] = 1
		for next_node in tree[cur_node]:
			if visited[next_node] == False:
				visited[next_node] = True
				subtree(next_node)
				size[cur_node] += size[next_node]
	```

### (1) Binary Tree
- A tree whose elements have at most 2 children.
	- Full binary tree: Every node has 0 or 2 children.
	- Complete binary tree: All the levels are completely filled except possibly the lowest one, which is filled from the left. (높은 레벨부터 아래로 차례대로 채워나감.)
		- Heap:
			- Min Heap: Parent <= Child
			- Max Heap: Parent >= Child
	- Binary search tree: Left <= Parent <= Right
### Quadtree
- Source: https://en.wikipedia.org/wiki/Quadtree
- A quadtree is a tree data structure in which each internal node has exactly four children. Quadtrees are the two-dimensional analog of octrees and are most often used to partition a two-dimensional space by recursively subdividing it into four quadrants or regions.
```python
from itertools import product

def quadtree(arr):
    if len(arr) == 1:
        return arr[0][0]
    else:
        n = len(arr)//2
        for i, j in product(range(len(arr)), range(len(arr))):
            if arr[i][j] != arr[0][0]:
                return f"({quadtree([i[:n] for i in arr[:n]]) + quadtree([i[n:] for i in arr[:n]]) + quadtree([i[:n] for i in arr[n:]]) + quadtree([i[n:] for i in arr[n:]])})"
        else:
            return arr[0][0]
```
### Octree
## Depth First Search
- Preorder Traverse: Parent -> Left -> Right
```python
def preorder(node):
    if node != None:
        left, right = tree[node]
        print(node)
        preorder(left)
        preorder(right)
```
- Inorder Traverse: Left -> Parent -> Right
```python
def inorder(node):
    if node != None:
        left, right = tree[node]
        inorder(left)
        print(node)
        inorder(right)
```
- Postorder Traverse: Left -> Right -> Parent
```python
def postorder(node):
    if node != None:
        left, right = tree[node]
        postorder(left)
        postorder(right)
        print(node)
```
## Breadth First search
- Level-Order Traverse: Level-order

## 7) Trie

# 5. Disjoint-Set(= Union-Find, Merge-Find)
- Two sets are said to be disjoint sets if they have no common elements.
- Source: https://www.geeksforgeeks.org/disjoint-set-data-structures/
- Problem: To find whether x and y belong to same group or not, i.e., to find if x and y are direct/indirect friends.
- Solution: Partitioning the individuals into different sets according to the groups in which they fall. This method is known as disjoint set data structure which maintains collection of disjoint sets and each set is represented by its representative which is one of its members.
- Approach
	- How to Resolve sets: Initially all elements belong to different sets. After working on the given relations, we select a member as representative. There can be many ways to select a representative, a simple one is to select with the biggest index.
	- ***Check if 2 persons are in the same group: If representatives of two individuals are same, then they’ll become friends.***
- Data Structures used
	- Array: An array of integers `parent`. If we are dealing with `n` items, `i`’th element of the array represents the parent of the `i`’th item. These relationships create one, or more, virtual trees.
	- Tree: It is a disjointset. If two elements are in the same tree, then they are in the same disjoint set. The root node of each tree is called the representative of the set. ***There is always a single unique representative of each set. A simple rule to identify representative is, if `i` is the representative of a set, then `parent[i]` equals to `i`. If `i` is not the representative of his set, then it can be found by traveling up the tree until we find the representative.***
- Operations
	- Find: Can be implemented by recursively traversing the parent array until we hit a node who is parent of itself.
	```python
	parent = [i for i in range(n)]
	size = [1 for i in range(n)]
	
	def find(x):
		# If `x` is the parent of itself
		if x == parent[x]:
			# Then `x` is the representative of this set.
			return x
		# If `x` is not the parent of itself, then `x` is not the representative of his set.
		else:
			# Else we recursively call `find()` on its parent.
			# Path Compression: It speeds up the data structure by compressing the height of the trees. We put `x` and all its ascendants directly under the representative of this set.
			parent[x] = find(parent[x])	
			return parent[x]
	```
	- Union: It takes, as input, two elements. And finds the representatives of their sets using the find operation, and finally puts either one of the trees (representing the set) under the root node of the other tree, effectively merging the trees and the sets.
	```python
	def union(x, y):
		# Find the representatives (or the root nodes) for the set that includes `x` and `y` respectively.
		x_rep = find(x)
		y_rep = find(y)
		if x_rep < y_rep:
			# Make the parent of `x`’s representative be `y`’s representative.
			parent[x_rep] = y_rep
	        size[y_rep] += size[x_rep]
		elif y_rep < x_rep:
			parent[y_rep] = x_rep
	        size[x_rep] += size[y_rep]
	```

# 6. Exhaustive Search

## 1) Brute-Force Attack

# 7. Recursion
- [Factorial](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/factorial.py):
- [Fibonacci sequence](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/fibonacci_sequence.py):
- [Tower of Hanoi](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/tower_of_hanoi.py):
- [Binary strings without consecutive 1's](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/binary_strings_without_consecutive_1s.py):

# 8. Sort
<details>
<summary>Click to expand</summary>

- Stable sorting algorithm: Sorting algorithm which maintains the relative order of records with equal keys (i.e. valuies).
- Comparison sorting algorithm:

## 1) [Quick Sort](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/quick_sort.py)
- Time Complexity:
	- Best case: $O(n\log n)$
	- Worst case: $O(n^2)$
	- Average case: $O(n\log n)$

## 2) [Merge Sort](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/merge_sort.py)
- Stable sorting algorithm.
- Comparison sorting algorithm.
- Time Complexity:
	- Merge Sort always performs the same number of comparisons and merging steps, regardless of whether the input array is sorted or not.
	- Best case: $O(n\log n)$
	- Worst case: $O(n\log n)$
	- Average case: $O(n\log n)$
- divide and conquer sorting algorithm

## 3) [Heap Sort](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/heap_sort.py)
- Best case: $O(n\log n)$
	- Worst case: $O(n\log n)$
	- Average case: $O(n\log n)$

## 4) [Bubble Sort](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/bubble_sort.py)
- Stable sorting algorithm.
- Comparison sorting algorithm.
- Time Complexity:
	- Best case: $O(n)$
	- Worst case: $(n - 1) + (n - 2) + \ldots + 1 = \frac{1}{2}n(n - 1) \rightarrow O(n^2)$
	- Average case: $O(n^2)$

## 5) [Insertion Sort](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/insertion_sort.py)
- 가장 직관적인 정렬 알고리즘.
- Time Complexity:
	- Best case: $O(n)$
	- Worst case: $O(n^2)$
	- Average case: $O(n^2)$
- Stable sorting algorithm.
- Comparison sorting algorithm.

## 6) [Selection Sort](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/selection_sort.py)
- Time Complexity: O(n^2)
- Not a stable sorting algorithm
- Comparison sorting algorithm.
<!-- 
## 7) Counting Sort
- Not a comparison sorting algorithm
- Time complexity: O(n + k)(k is the range of elements of the array)
- It assumes that all the elements of the array are positive integers.
```python
maxim = max(arr)
count = {i:0 for i in range(1, maxim + 1)}

for elm in arr:
    count[elm] += 1

new_arr = list()
for k, v in count.items():
    for _ in range(v):
        new_arr.append(k)
``` -->
</details>

# 9. Coordinate Compression
- Source: https://medium.com/algorithms-digest/coordinate-compression-2fff95326fb
- Coordinate compression is a technique to map a large set of points to a smaller range by removing gaps and/or redundant information. By compressing the points to a smaller range, we can save considerable time and memory.
- Coordinate Compression In 1D
	- The simplest example of coordinate compression is sorting a 1D array. Suppose we have an array `A` of size `N`. After sorting, the element `A[i]` is mapped to index `i`. This way, elements that were in an arbitrary range are now mapped to the range `[0, N - 1]` and yet their relative ordering is preserved.
	```python
	coors = [[i, coors[i]] for i in range(N)]
    coors = sorted(coors, key=lambda x:x[1])

    j = 0
    for i in range(N - 1):
        coors[i].append(j)
        if coors[i][1] != coors[i + 1][1]:
            j += 1
    coors[i + 1].append(j)

    print(*[i[2] for i in sorted(coors, key=lambda x:x[0])])
	```

# 10. Dynamic Programming
- Divide-and-Conquer & Memoization
```python
mem = dict()
def func(n):
	if n not in mem:
		mem[n] = ...
	return mem[n]
```
- [0-1 Knapsack Problem](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/0-1_knapsack_problem.py):
- [LIS (Longest Increasing Subsequence)](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/longest_increasing_subsequence.py):
	- Time complexity: $O(n^2)$
- [LCS (Longest Common Subsequence)](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/longest_common_subsequence.py):
	- Time complexity: $O(nk)$
- [Longest common substring](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/longest_common_substring.py):
	- Time complexity: $O(nk)$
- [Prefix sum](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/prefix_sum.py):
	- Time complexity:

# 11. Greedy Algorithms
- Source: https://www.geeksforgeeks.org/greedy-algorithms/
- Greedy is an algorithmic paradigm that builds up a solution piece by piece, always choosing the next piece that offers the most obvious and immediate benefit. So the problems where choosing locally optimal also leads to global solution are best fit for Greedy.
- Fractional Knapsack Problem

# 12. Two-Pointers
- [Longest substring without repeating characters](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/longest_substring_without_repeating_characters.py):
	- Time complexity: $O(n)$
<!-- - Time complexity: O(n)
	```python
	arr = sorted(arr)

	left = 0
	right = len(arr) - 1
	while left < right:
		if func(left, right) == x:
			left += 1
			right -= 1
			...
		elif func(left, right) > x:
			right -= 1
			...
		elif func(left, right) < x:
			left += 1
			...
	```
	```python
	arr += [0]
	
	left = 0
	right = 0
	while left <= len(arr) - 1 and right <= len(arr) - 1:
		if func(left, right) == x:
			left += 1
			right += 1
			...
		elif func(left, right) > x:
			left += 1
			...
		elif func(left, right) < x:
			right += 1
			...
	``` -->

# 13. Binary Search
- Time complexity: O(logn)
```python
arr = sorted(arr)

# `left`: Possible minimum value
# `right`: Possible maximum value
left = 0
right = len(arr) - 1
while left <= right:
    mid = (left + right)//2

    if arr[mid] == tar:
        ...
		break
    elif arr[mid] > tar:
        right = mid - 1
    else:
        left = mid + 1
```
```python
from bisect import bisect_left, bisect_right

arr = sorted(arr)

print(... if bisect_right(arr, tar) == bisect_left(arr, tar) else ...)
```
```python
def bisect_left(arr, tar):
    left = 0
    right = len(arr)
    while left < right:
        mid = (left + right)//2

        if arr[mid] < tar:
            left = mid + 1
        else:
            right = mid
    return left
    
def bisect_right(arr, tar):
    left = 0
    right = len(arr)
    while left < right:
        mid = (left + right)//2

        if arr[mid] > tar:
            right = mid
        else:
            left = mid + 1
    return left
```

# 14. Parametric Search
- Source: https://velog.io/@lake/%EC%9D%B4%EB%B6%84%ED%83%90%EC%83%89-%ED%8C%8C%EB%9D%BC%EB%A9%94%ED%8A%B8%EB%A6%AD-%EC%84%9C%EC%B9%98Parametric-Search
- ***Parametric search is a technique for transforming an optimization algorithm (find the best solution) into a decision algorithm (does this optimization problem have a solution with quality better than some given threshold?).***
```python
arr = sorted(arr)

# The initial value for `left` should be equal to the possible minimum value for the answer.
# The initial value for `right` should be equal to the possible maximum value for the answer.
# The initial value for `ans` should be among the values that cannot be the answer.
while left <= right:
    mid = (left + right)//2
```
- 구하고자 하는 값이 최대값이고 `func()`가 Increasing function일 때:
```python
if func(mid) <= tar:
	left = mid + 1
	ans = mid
else:
	right = mid - 1
```
- 구하고자 하는 값이 최대값이고 `func()`가 Decreasing function일 때:
```python
if func(mid) >= tar:
	left = mid + 1
	ans = mid
else:
	right = mid - 1
```
- 구하고자 하는 값이 최소값이고 `func()`가 Increasing function일 때:
```python
if func(mid) >= tar:
	right = mid - 1
	ans = mid
else:
	left = mid + 1
```
- 구하고자 하는 값이 최소값이고 `func()`가 Decreasing function일 때:
```python
if func(mid) <= tar:
	right = mid - 1
	ans = mid
else:
	left = mid + 1
```

# 15. String-Searching Algorithms

## 1) KMP Algorithm(Knuth-Morris-Pratt Algorithm)
- Source: https://blog.encrypted.gg/857
- Time complexity: O(n + k) (where `n` and `k` are the lengths of string `s` and pattern `p` respectively).
- The KMP algorithm is an algorithm that is used to search for `p` in `s`.
- Failure Function
	- Time complexity: O(k)
	- The mapping of the index `j` to the LPS(Longest proper Prefix which is also proper Suffix) of `p[:j + 1]`.
	- This function is based on the fact that when a mismatch occurs, all the previous characters match correctly.
- String-Search
	- Time complexity: O(n)

## 2) Rabin-Karp Algorithm
- Source: https://www.programiz.com/dsa/rabin-karp-algorithm
- A string `s` is taken and checked for the possibility of the presence of the pattern `p`. If the possibility is found then, character matching is performed.
- Rolling Hash
	- Source: https://en.wikipedia.org/wiki/Rolling_hash
	- A rolling hash is a hash function where the input is hashed in a window that moves through the input.
	- A few hash functions allow a rolling hash to be computed very quickly—the new hash value is rapidly calculated given only the old hash value, the old value removed from the window, and the new value added to the window—similar to the way a moving average function can be computed much more quickly than other low-pass filters.
	```python
	hash = ord(s[0])*(d**(len(p) - 1)) + ord(s[1])*(d**(len(p) - 2)) + ... + ord(s[len(p) - 1])
	```

# 16. Hash
<details>
<summary>Click to expand</summary>
- source: https://wangin9.tistory.com/entry/hash
- 사람은 누구든지 "이름" = "홍길동", "생일" = "몇 월 몇 일" 등으로 구분할 수 있다. 파이썬은 영리하게도 이러한 대응 관계를 나타낼 수 있는 자료형을 가지고 있다. 요즘 사용하는 대부분의 언어들도 이러한 대응 관계를 나타내는 자료형을 갖고 있는데, 이를 연관 배열(Associative array) 또는 해시(Hash)라고 한다. 파이썬에서는 이러한 자료형을 딕셔너리(Dictionary)라고 하는데, 단어 그대로 해석하면 사전이라는 뜻이다. 즉, people이라는 단어에 "사람", baseball이라는 단어에 "야구"라는 뜻이 부합되듯이 딕셔너리는 Key와 Value라는 것을 한 쌍으로 갖는 자료형이다. 예컨대 Key가 "baseball"이라면 Value는 "야구"가될 것이다. 딕셔너리는 리스트나 튜플처럼 순차적으로(sequential) 해당 요소값을 구하지 않고 Key를 통해 Value를 얻는다. 이것이 바로 딕셔너리의 가장 큰 특징이다. baseball이라는 단어의 뜻을 찾기 위해 사전의 내용을 순차적으로 모두 검색하는 것이 아니라 baseball이라는 단어가 있는 곳만 펼쳐 보는 것이다.
- 규모가 큰 데이터를 비교해야할 때 list 형태로 순차적으로 비교하는 것은 시간이 O(n2) 걸리게 된다. 이때 hash 를 사용하게 되면 시간을 절약할 수 있다.
- 해쉬란 임이의 크기를 가진 데이터를 고정된 데이터의 크기로 변환시키는 것을 말한다. 이를 이용해 특정한 배열의 인덱스나 위치, 위치를 입력하고자 하는 데이터의 값을 이용해 저장하거나 찾을 수 있다. 기존에 사용했던 자료구조들은 탐색이나 삽입에 선형시간이 걸리기도 했던 것에 비해, 해쉬를 이용하면 즉시 저장하거나 찾고자 하는 위치를 참조할 수 있으므로 더욱 빠른 속도로 처리할 수 있다.
- Direct Addressing Table
	- Direct Addressing Table은 key-value 쌍의 데이터를 배열에 저장할 key 값을 직접적으로 배열의 인덱스로 사용하는 방법이다. 예를 들면 400인 데이터가 있다면 이는 배열의 인덱스가 400인 위치에 키 값을 저장하고 포인터로 데이터를 연결한다. 똑같은 키 값이 존재하지 않는다고 가정하면 삽입 시에는 각 키마다 자신의 공간이 존재하므로 그 위치에다 저장하면 되고 삭제 시에는 해당 키의 위치에 NULL값을 넣어주면 된다. 탐색 시에는 해당 키의 위치를 그냥 찾아가서 참조하면 된다. 찾고자 하는 데이터의 key만 알고 있으면 즉시 위치 찾는 것이 가능하므로 탐색, 저장, 삭제, 갱신은 모두 선형시간 O(1)로 매우 빠른 속도로 처리가 가능하다. 다만 key 값의 최대 크기만큼 배열이 할당 되기 때문에 크기가 매우 크고 저장하고자 하는 데이터가 적다면 공간을 많이 낭비할 수 있다는 단점이 있다.
- Hash Table
	- Hash Table은 key-value 쌍에서 key 값 테이블에 저장할 때 Direct Addressing Table과 달리 함수를 이용해 key값의 계산을 수행한 후 그 결과값을 배열의 인덱스로 사용하여 저장하는 방식이다. 여기서 key 값을 계산하는 함수는 해쉬 함수(Hash Function)이라고 부르며 해쉬 함수는 입력으로 key를 받아서 배열의크기-1 사이의 값을 출력한다. 해쉬의 첫 정의대로 임의의 숫자를 배열의 크기만큼으로 변환시킨 것이다. 이경우 k 값이 h(k)로 해쉬되었다고 하며 h(k)는 k의 해쉬값이라고 한다.
</details>

# 17. Bitmask
<details>
<summary>Click to expand</summary>
- Source: https://en.wikipedia.org/wiki/Mask_(computing)
- In computer science, a mask or bitmask is data that is used for bitwise operations, particularly in a bit field. Using a mask, multiple bits in a byte, nibble, word etc. can be set either on, off or inverted from on to off (or vice versa) in a single bitwise operation.
```python
bitmask = (1 << <<Position>>)

# Getting a bit: To read the value of a particular bit on a given position.
<<Target>> & bitmask

# Setting a bit
<<Target>> | bitmask

# Unsetting a bit: To clear a bit, you want to copy all binary digits while enforcing zero at one specific index.
<<Target>> & ~bitmask

# Toggling a bit
<<Target>> ^ bitmask
```
</details>

# 18. Number Theory
- [Sieve of Eratosthenest](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/sieve_of_eratosthenest.py)
- [Prime factorization](https://github.com/KimRass/Algorithm-Coding-Test/blob/main/prime_factorization.py)
- Greatest Common Divisor
- Least Common Multiple
- Combination(= Binomial Coefficient)
	- The number of `k`-combinations If the set has `n` elements

<!-- 
```python
import heapq

heapq.heappush(<<Heap Object>>, <<Element>>)
heapq.heqppop(<<Heap Object>>)

heapq.heappush(<<Heap Object>>, -<<Element>>)
heapq.heqppop(<<Heap Object>>)
```
```python
heapq.nsmallest()
heapq.nlargest()
# Transform a list into a heap in linear time. s(In-place function)
heapq.heapify()
``` -->
