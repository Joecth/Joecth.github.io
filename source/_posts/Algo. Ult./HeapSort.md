---
layout: post
categories: AlgoUlt.
tag: [] 
date: 2019-09-25
---





# HeapSort

```python
print("Hello World!")


def sift_down(tree, n, node):   # cross check: https://youtu.be/j-DqQcNPGbE?t=1157
    # print(node)
    left_node = node * 2 + 1
    right_node = node * 2 + 2
    node_of_max = node
    if left_node < n and tree[left_node] > tree[node_of_max]:
        node_of_max = left_node
    if right_node < n and tree[right_node] > tree[node_of_max]:
        node_of_max = right_node
    if node_of_max != node:
        tree[node], tree[node_of_max] = tree[node_of_max], tree[node]
        sift_down(tree, n, node_of_max)
    
        
def build_heap(tree, n):    # cross check: https://youtu.be/j-DqQcNPGbE?t=1336
    last_node = n - 1
    parent = (last_node - 1) // 2
    i = parent
    while i >= 0:
        sift_down(tree, n, i)
        i -= 1

        
def heap_sort(tree, n):     # ref to : https://youtu.be/j-DqQcNPGbE?t=1601
                                # 2) sift_down's name: https://rust-algo.club/sorting/heapsort/
    build_heap(tree, n)
    i = n - 1
    while i >= 0:
        tree[i], tree[0] = tree[0], tree[i]
        sift_down(tree, i, 0)
        i -= 1

arr = [4, 10, 3, 5, 1, 2]
n = len(arr)
print(f'arr original: {arr}')
# sift_down(arr, 0, n-1, 0)
sift_down(arr, n, 0)
print(f'arr after sift_down: {arr}')

arr2 = [2, 5, 3, 1, 10, 4]
print(f'arr2 original (A BETTER CASE): {arr2}')
build_heap(arr2, len(arr2))
print(f'arr2 after build_heap: {arr2}')

arr2 = [2, 5, 3, 1, 10, 4]
heap_sort(arr2, len(arr2))
print(f'arr2 after heap_sort: {arr2}')
```