#### DFS
```py
visited = set()

def dfs(node):
    if node in visited:
        return 

    visited.add(node)

    for nxt in node.children:
        if nxt not in visited:
            dfs(nxt)
```

#### BFS
```py
def bfs(graph, start, end):
    visited = set()
    queue = []
    queue.append([start])

    while queue:
        node = queue.pop()
        visited.add(node)

        process(node)

        nodes = generate_related_nodes(node)
        queue.push(nodes)
```

#### binary search
```py
left, right = 0, len(array) - 1

while left <= right:
    mid = (left + right) // 2   

    if array[mid] == target:
        # break or return result

    elif array[mid] < target:
        left = mid + 1
    else:
        right = mid - 1

```