### Fibonacci Heaps: A Detailed Explanation

A **Fibonacci Heap** is a type of priority queue that consists of a collection of trees, which are structured to allow for efficient merging and decrease operations. It is named after the Fibonacci numbers because the trees in a Fibonacci heap have a relationship with the Fibonacci sequence.

#### Key Features of Fibonacci Heaps

1. **Structure**: 
   - A Fibonacci heap is a collection of tree-structured nodes. Each tree follows the min-heap property, meaning the key of a node is greater than or equal to the key of its parent.
   - Nodes can have multiple children, and there is no specific order among siblings.

2. **Lazy Merging**: 
   - Fibonacci heaps allow for lazy merging of heaps, which means they do not immediately restructure trees when merging; instead, they maintain a pointer to the merged trees.

3. **Amortized Analysis**: 
   - The performance of operations is analyzed in terms of amortized time, meaning the average time per operation over a sequence of operations is considered.

4. **Potential Function**: 
   - The analysis is often done using a potential function, which measures the number of trees in the heap and the number of marked nodes.

#### Operations on Fibonacci Heaps

1. **Insertion**: Add a new node to the root list of the heap.
  
2. **Find Minimum**: Return the minimum node in the root list.

3. **Union**: Merge two Fibonacci heaps by concatenating their root lists.

4. **Extract Minimum**: Remove the minimum node, then consolidate the trees to maintain the heap property.

5. **Decrease Key**: Decrease the key of a node and restructure the heap if necessary.

6. **Delete**: Decrease the key of a node to negative infinity and then extract the minimum.

#### Implementation of Fibonacci Heaps in Python

Here’s a sample implementation of a Fibonacci Heap in Python:

```python
class FibonacciNode:
    def __init__(self, key):
        self.key = key
        self.degree = 0  # Number of children
        self.parent = None  # Parent node
        self.child = None  # First child
        self.is_marked = False  # Marked status
        self.next = self  # Points to the next node
        self.prev = self  # Points to the previous node

class FibonacciHeap:
    def __init__(self):
        self.min_node = None  # Minimum node
        self.num_nodes = 0  # Total number of nodes

    def insert(self, key):
        # Insert a new key into the heap
        new_node = FibonacciNode(key)
        if not self.min_node:
            self.min_node = new_node
        else:
            self._link(self.min_node, new_node)
            if new_node.key < self.min_node.key:
                self.min_node = new_node
        self.num_nodes += 1

    def _link(self, min_node, new_node):
        # Link two nodes together
        new_node.prev = min_node.prev
        new_node.next = min_node
        min_node.prev.next = new_node
        min_node.prev = new_node

    def find_minimum(self):
        # Find the minimum node in the heap
        return self.min_node.key if self.min_node else None

    def extract_minimum(self):
        # Remove the minimum node from the heap
        min_node = self.min_node
        if min_node:
            if min_node.child:
                child = min_node.child
                while True:
                    next_child = child.next
                    self._link(min_node, child)
                    child.parent = None
                    if next_child == min_node.child:
                        break
                    child = next_child
            min_node.prev.next = min_node.next
            min_node.next.prev = min_node.prev
            if min_node == min_node.next:
                self.min_node = None
            else:
                self.min_node = min_node.next
                self._consolidate()
            self.num_nodes -= 1
        return min_node.key

    def _consolidate(self):
        # Consolidate the trees in the heap
        max_degree = int(self.num_nodes ** 0.5) + 1
        temp = [None] * max_degree

        current = self.min_node
        nodes = []
        while True:
            nodes.append(current)
            current = current.next
            if current == self.min_node:
                break

        for node in nodes:
            degree = node.degree
            while temp[degree]:
                other = temp[degree]
                if node.key > other.key:
                    node, other = other, node
                self._link(node, other)
                temp[degree] = None
                degree += 1
            temp[degree] = node

        self.min_node = None
        for node in temp:
            if node:
                if not self.min_node:
                    self.min_node = node
                else:
                    self._link(self.min_node, node)
                    if node.key < self.min_node.key:
                        self.min_node = node

    def decrease_key(self, node, new_key):
        # Decrease the value of a key
        if new_key > node.key:
            raise ValueError("New key is greater than current key")
        node.key = new_key
        parent = node.parent
        if parent and node.key < parent.key:
            self._cut(node)
            self._link(self.min_node, node)
        if node.key < self.min_node.key:
            self.min_node = node

    def _cut(self, node):
        # Cut the node from its parent
        parent = node.parent
        if parent:
            if node.next == node:
                parent.child = None
            else:
                if parent.child == node:
                    parent.child = node.next
                node.prev.next = node.next
                node.next.prev = node.prev
            parent.degree -= 1
            node.parent = None
            node.is_marked = False
            self._link(self.min_node, node)

    def delete(self, key):
        # Delete a key from the heap
        node = self.find_node(key)
        if node:
            self.decrease_key(node, float('-inf'))
            self.extract_minimum()

    def find_node(self, key):
        # Find a node with the given key
        return self._find_node(self.min_node, key)

    def _find_node(self, node, key):
        if not node:
            return None
        if node.key == key:
            return node
        found_node = self._find_node(node.child, key)
        if found_node:
            return found_node
        if node.next != self.min_node:
            return self._find_node(node.next, key)
        return None

# Example Usage
if __name__ == "__main__":
    fibonacci_heap = FibonacciHeap()
    fibonacci_heap.insert(10)
    fibonacci_heap.insert(20)
    fibonacci_heap.insert(5)
    fibonacci_heap.insert(6)
    fibonacci_heap.insert(12)
    print("Minimum:", fibonacci_heap.find_minimum())  # Output: 5
    print("Deleted Minimum:", fibonacci_heap.extract_minimum())  # Output: 5
    print("New Minimum:", fibonacci_heap.find_minimum())  # Output: 6
    fibonacci_heap.delete(20)
```

### Summary

Fibonacci Heaps are advanced data structures that allow for more efficient operations than other heap types, particularly when it comes to merging heaps and decreasing keys. Their amortized time complexity for various operations makes them suitable for algorithms that require frequent priority queue manipulations, such as Dijkstra's and Prim's algorithms.