# Data Structures

## 0,. General Notes
1. **Iterating through a string**
  ```
  for (char ch : exampleString.toCharArray()){ System.out.println(ch);}
  ```
2. **Creating an array**
  ```
  int[] arr = new int[] {1,2,3,4,5};
  ```
3. **Creating a list from array**
  ```
  List<Integer> intList = Arrays.asList(new Integer[] {1,2,3,4,5});
  ```
4. **Common implementation [https://docs.oracle.com/javase/tutorial/collections/implementations/summary.html]**
  - `Set` => `HashSet` implementation.
  - `List` => `ArrayList` implementation.
  - `Map` => `HashMap` implementation.
  - `Queue` => `LinkedList` implementation.
  - `Deque` => `ArrayDeque` implementation.
  - `Min-Heap`  => `PriorityQueue<>()` implementation.
  - `Max-Heap`  => `PriorityQueue<>(Collections.reverseOrder())` implementation.
5. **Common methods**
  - `Stack` => `.push(E)`, `.pushAll(Collection<E>)`, `.pop()`
  - `String` => `.subString(start, endNotInclusive)`, `.charAt(int)`, `.toCharArray()`
  - `LinkedList` (queue) => `.add(E)`, `.addAll(Collection<E>)`, `.poll()`
  - `PriorityQueue` => `.add(E)`, `.addAll(Collection<E>)`, `.poll()`

## 1. Arrays
1. **Intro [x]**
   - Notes
      - Cannot create without specifying size. Size can be a variable.
      - When asked for sum-of-subarray
        - insert start-to-current sum for every element into another array, with start of array as 0, 1st = arr[0], 2nd = 1st + arr[1].. so on
        - sum-of-subarray from i to j can be got by arr[j] - arr[i-1]
      - given two arrays, find k max/min pair sums, picking one element from each array.
        - simple soln [https://www.geeksforgeeks.org/k-maximum-sum-combinations-two-arrays/]
2. **Rotation [x]**
   - interesting problems
     - Find pivot in a rotated sorted array / Find rotation count rotated sorted array - [https://www.geeksforgeeks.org/find-rotation-count-rotated-sorted-array/]
     - Find maximum value of Sum( i*arr[i]) with only rotations on given array allowed - [https://www.geeksforgeeks.org/find-maximum-value-of-sum-iarri-with-only-rotations-on-given-array-allowed/]
3. **Rearrangement [x]**
4. **Order Statistics [x]**
   - Interesting
     - find the k largest/smallest elements.
       - Use heap of size=k. largest = minHeap, smallest = maxHeap
     - Longest Contiguous Sum
       - keep track of maxSumSoFar
       - keep track of maxSumTillCurrent
       - if maxSumTillCurrent < 0, reset the subarray.
         - since adding any number to this maxSumTillCurrent will be less than starting the subarray after this.
     - number of subarrays with even sum
       - keep track of contiguous sum in cs[i]
       - a subarray from i to j has even sum if
         - cs[j] and cs[i] are even
         - cs[j] and cs[i] are odd
         - counting left to right, using combinatrics ans
           - numberEvenSumsC2 = NES!/[2! (NES-2)!]
           -  numberOddSumsC2 = NOS!/[2! (NOS-2)!]
     - next largest combination of digits - [https://www.geeksforgeeks.org/find-next-greater-number-set-digits/] 
5. **Range Queries [x]**
6. **Searching and Sorting [ ]**  => _come back to it_
7. **Optimization Problems [ ]**  => _come back to it_
8. **Matrix [ ]**  => _come back to it_

## 2. Stacks
1. **Introduction [x]**
1. **Design and Implementation [x]**
1. **Standard Problems [x]**
1. **Operations [x]**
 
## 3. Queues
1. **Introduction [x]**
   - Notes
     - Common implementation of queue is LinkedList<>
2. **Design and Implementation [x]**
3. **Standard Problems [x]**
4. **Operations [x]**

## 4. Linked List
   - notes
     - fast pointer, slow pointer approach
1. **Singly Linked List [x]**
   - interesting problems
     - find loop
       - use hash set to keep track of visited node
       - floyds cycle algo - slow pointer moves singly, fast pointer moves two nodes. if they meet before end, cycle.
       - to find length, use floyd's algo to find a node in the loop. go round the loop till same node is found.
2. **Circular Linked List [x]**
3. **Doubly Linked List [ ]**  => _come back to it_

## 5. Hash Tables - 136 [x]
1. **Basics - 6**
1. **Easy - 62**
1. **Intermediate - 43**
   - Interesting Problems
     - [https://www.geeksforgeeks.org/find-if-there-is-a-subarray-with-0-sum/]
     - 
2. **Hard - 25**
## 6. Heaps - 48 [x]
  - PriorityQueue is a good implementation of min-heap
  - PriorityQueue(capacity, Comparators.reverseOrder) is an implemenation of max-heap.
  - Check median of running numbers - [https://www.geeksforgeeks.org/median-of-stream-of-integers-running-integers/]
  - BuildHeap
    - Bottom-up
    - called when building the heap the first time. uses heapify internally
  - Heapify
    - top-Down
    - called when inserting a new 

## 7. Trees - 161
1. **Introduction - 17 [x]**
   - Balancing AVL - [https://www.geeksforgeeks.org/avl-tree-set-1-insertion/]
2. **Traversals - 32 [x]**
3. **Construction & Conversion - 35 [x]**
4. **Checking & Printing - 34**
5. **Summation - 33**
6. **Longest Common Ancestor - 12**

## 8. Binary Search Trees - 60
1. **Basic**
1. **Construction and Conversion - 18**
1. **Check and Smallest/Largest Element - 42**

## 9. Graphs - 165
1. **Introduction, DFS and BFS - 51**
    - when looking for paths/cycles, use DFS with backtracking, ie, DFS where, at the end of a node's processing, it is marked as *unvisited*
1. **Graph Cycle - 15**
1. **Topological Sorting - 9**
  - Kahn's algo (V + E)
    - find all nodes with 0 incoming edges (in-degree). Add em to a queue
    - while queue is not empty
      - dequeue node.
      - print node.
      - decrement in-degree of unvisited nodes adj to this node
      - if the in-degree of any of these nodes becomes zero, add to queue.

2. **Minimum Spanning Tree - 10**
- Checkout algo description videos - [https://www.youtube.com/channel/UCzDJwLWoYCUQowF_nG3m5OQ]
- Kruskal's algo (greedy) (E log E)
  - sort edges by weight
  - iterate through edges and add them to MST as long as a cycle is not formed.
  - cycle detection. check if both nodes of the edge are part of MST. if true => cycle
  - stop when all vertices are visited

- Prim's algo (greedy) (V*V)
  - pick random node. start MST from here. Mark it as visited.
  - till all nodes are visited, repeat
    - list all edges between nodes in visited and nodes not in visited.
    - sort this list.
    - add the smallest edge.

1. **BackTracking - 8**
2. **Shortest Paths - 21**
- Dijkstra's algo (V log V)
  - select starting node. mark it as visited.
  - update dist table if dest-current-adjNode < existing value in dist table
    - ie. [src -> intermediateNode -> dest] < [src -> currentPath -> dest]
  - mark node as visited.
  - while there are unvisited nodes, choose the first unvisited node that is closest to the src.

- Bellman Ford Algo (works with negative weights) (non-greedy) (V.E)
  - list all nodes with the startNode first.
  - repeat (numberOfNodes - 1) times
    - update dist table if dest-current-adjNode < existing value in dist table
      - ie. [src -> intermediateNode -> dest] < [src -> currentPath -> dest]
    - stop if dist table is not updated.

- Floyd-Warshall's algo (works with negative weights + gives distance between all pairs on nodes)
  - form 3 for loops (i,j,k) iterating through all verticies
  - check if dist(i,j)+ dist(j,k) < dist(i,k)
    - update dist table with new distance in [i,k]
  - NOTE: if distance from a node to itself is less than 0 in this algo, a negative cycle was detected.
1. **Connectivity - 40**
2. **Maximum Flow - 11**


# Algorithms

## 1. Analysis of Algorithms
## 2. Searching and Sorting - 31
  - n^2
    - Insertion Sort - assume array to left is sorted and insert current element into the sorted array at the correct location.
    - Selection Sort - assume array to left is sorted. Select smallest element in unsorted section and append it(swap with first unsorted element) to sorted array.
    - Bubble sort - array to RIGHT is sorted (max). Compare adj elements in unsorted array. swap if left > right.
  - nlogn
    - merge sort
    - heap sort
      - build-max-heap = n
      - heapify (called n times) = logn
      - n + nlogn = nlogn
    - quick sort (worst case n^2 with bad pivot)
      - choose pivot.
      - move elements less than pivot to left, greater than pivot to right
      - repeat process for the left and right sub-arrays.
    - bucket sort (with insertion sort)

## 3. Greedy Algorithms - 13
## 4. Dynamic Programming - 56
## 5. Pattern Searching - 14
## 6. Other String Algorithms - 3
## 7. Backtracking - 10
## 8. Divide and Conquer - 7
## 9. Geometric Algorithms - 8
## 10. Mathematical Algorithms - 62
## 11. Bit Algorithms - 40
## 12. Graph Algorithms - 45
## 13. Randomized Algorithms - 11
## 14. Branch and Bound - 6

# Programming Concepts

## 1. Threads
### concurrent execution
  - [https://www.geeksforgeeks.org/different-approaches-to-concurrent-programming-in-java/]
  - parallel execution within a thread-pool
  - ```
    java.util.concurrent.Executors
    ExecutorService taskList = Executors.newFixedThreadPool(poolSize);
    taskList.execute(someRunnable)
    ```

### daemon threads
  - low priority background threads
  - `thread.setDaemon(true)`
  - [https://www.geeksforgeeks.org/difference-between-daemon-threads-and-user-threads-in-java/]

### thread function
  - `Thread.yield()`
    - yields control of the processor core to another thread. Effectively a pause.
    - If no other thread of higher or equal priority is found, control of the core will return to the calling thread.
  - `Thread.sleep(timeInMs)`
    - pauses use of the processor core, similar to yield. Gives up control of the processor core.
    - if there are no other threads, the thread still does not execute. Processor core may enter idle state.
  - `anotherThread.join(waitTimeInMs)`
    - callingThread waits for `waitTimeinMs` for `anotherThread` to complete.
    - if no `waitTimeInMs` is specified, indefinitely wait.

## 2. Synchronization
  - basic synchronization = [https://www.geeksforgeeks.org/method-block-synchronization-java/]
  - reentrant locks = [https://www.geeksforgeeks.org/reentrant-lock-java/]
## 3. Deadlocks
  - [https://www.geeksforgeeks.org/operating-system-process-management-deadlock-introduction/]