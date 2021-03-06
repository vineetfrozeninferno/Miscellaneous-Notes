# Data Structures

- [http://blog.gainlo.co/index.php/category/coding-interview-questions/]

## 0. General Notes
1. **Iterating through a string**
    ```
    for (char ch : exampleString.toCharArray()){ System.out.println(ch);}
    ```
2. **Creating an array**
    ```
    int[] arr = new int[] {1,2,3,4,5};
    ```
    ```
    int[] anArray = new int[10];
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
  - `String` => `.substring(start, endNotInclusive)`, `.charAt(int)`, `.toCharArray()`, `.length()`
  - `Array` => `.length` (no parentheses)
  - `LinkedList` (queue) => `.add(E)`, `.addAll(Collection<E>)`, `.poll()`
  - `PriorityQueue` => `.add(E)`, `.addAll(Collection<E>)`, `.poll()`
  - `Map` => `.keySet()`, `.values()`, `containsKey(Type key)`, [`.entrySet()` (has Map.Entry objects that have `.getKey()`, `.getValue()`)]
6. **Comparable Interface**
  - `int compareTo(Type other)`
7. **OverlappingRanges**
  - [https://stackoverflow.com/questions/325933/determine-whether-two-date-ranges-overlap/325964#325964]
  - for ranges [(s1,e1),(s2,e2)] => overlap if `s1<=e2 &&s2<=e1`

## 1. Arrays
1. **Intro [x]**
   - **Notes**
      - Cannot create without specifying size. Size can be a variable.
      - When asked for sum-of-subarray
        - insert start-to-current sum for every element into another array, with start of array as 0, 1st = arr[0], 2nd = 1st + arr[1].. so on
        - sum-of-subarray from i to j can be got by arr[j] - arr[i-1]
      - given two arrays, find k max/min pair sums, picking one element from each array.
        - simple soln [https://www.geeksforgeeks.org/k-maximum-sum-combinations-two-arrays/]
2. **Rotation [x]**
   - **interesting problems**
     - Find pivot in a rotated sorted array / Find rotation count rotated sorted array - [https://www.geeksforgeeks.org/find-rotation-count-rotated-sorted-array/]
     - Search in rotated sorted array - [http://blog.gainlo.co/index.php/2017/01/12/rotated-array-binary-search/](http://blog.gainlo.co/index.php/2017/01/12/rotated-array-binary-search/)
        - find pivot (log N)
        - binary search from start to pivot (log N)
        - binary search from pivot to end (log N)
     - Find maximum value of Sum( i*arr[i]) with only rotations on given array allowed - [https://www.geeksforgeeks.org/find-maximum-value-of-sum-iarri-with-only-rotations-on-given-array-allowed/]
3. **Rearrangement [x]**
  - **interesting problems**
    - Find the largest number possible given set of digits **(time-comp=O(n))**
      - [details-in-comment](http://blog.gainlo.co/index.php/2017/01/20/arrange-given-numbers-to-form-the-biggest-number-possible/#comment-6125)
      - maintain frequency-table of digits. table can have only entries from 0-9.
    - next largest combination of digits - [https://www.geeksforgeeks.org/find-next-greater-number-set-digits/], [second half of post](http://blog.gainlo.co/index.php/2017/01/20/arrange-given-numbers-to-form-the-biggest-number-possible/)

4. **Order Statistics [x]**
   - **Interesting**
     - find the k largest/smallest elements.
       - Use heap of size=k. largest = minHeap, smallest = maxHeap
     - Longest Contiguous Sum
       - keep track of maxSumSoFar
       - keep track of maxSumTillCurrent
       - if maxSumTillCurrent < 0, reset the subarray.
         - since adding any number to this maxSumTillCurrent will be less than starting the subarray after this.
     - Longest Increasing Subsequence - [https://www.geeksforgeeks.org/dynamic-programming-set-3-longest-increasing-subsequence/]
     - Longest Common Subsequence - [https://www.geeksforgeeks.org/dynamic-programming-set-4-longest-common-subsequence/]
     - number of subarrays with even sum
       - keep track of contiguous sum in cs[i]
       - a subarray from i to j has even sum if
         - cs[j] and cs[i] are even
         - cs[j] and cs[i] are odd
         - counting left to right, using combinatrics ans
           - numberEvenSumsC2 = NES!/[2! (NES-2)!]
           -  numberOddSumsC2 = NOS!/[2! (NOS-2)!]
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
1. Notes
  - Consistent Hashing [link](https://towardsdatascience.com/consistent-hashing-simplified-7fe4e512324)
    - used when a hashMap is distributed across multiple servers
    - **`% number-of-server`**.
      - +ve = very simple
      - -ve = if number-of-servers changes due to server-addition or server-failure, most of the data has to be repositioned
        - eg - if there are 3 servers, and range of hash-output is 6.
          - server0 => 0, 3
          - server1 => 1, 4
          - server2 => 2, 5
          when one server goes down, the modulo drops to 2
          - server0 => 0, 2, 4
          - server1 => 1, 3, 5
        keys 3 and 4 was moved even though there was nothing wrong with server0
    - **`consistent hashing`**
      - +ve - resilient to changes in number of servers
      - -ve - more complex
      - eg - if there are 3 servers, and range of hash-output is 6.
        - server0 takes any value less than 6/3 = 2
        - server1 takes any value less than 2*6/3 = 4
        - server1 takes any value less than 3*6/3 = 6
        so,
          - server0 => 0, 1
          - server1 => 2, 3
          - server2 => 4, 5
        - when server2 goes down, keys from server2 wrap arnd and get stored onto server0.
        - when server0 goes down, keys from server0 are stored onto server1

1. **Basics - 6**
1. **Easy - 62**
1. **Intermediate - 43**
   - Interesting Problems
     - Pairs with a given sum [https://www.geeksforgeeks.org/count-pairs-with-given-sum/]
       - Add all the numbers to a HashSet.
       - Before adding check if `(sum - number)` already exists in the hashSet. If yes, increment `pairsCount`.
     - Pairs that are divisible by given number K
       - The Math
         - if a number is divisble by K, `number = aK`
         - if `number % K = r`, `number = aK + r`, where `0 <= r < K`
         - consider 2 numbers `(aK + r1)` and `(bK + r2)`
           - if their sum were divisible by K, `aK + r1 + bK + r2 = (a+b+1)K`, since `r1 + r2 cant be greater than 2(K-1) or 2K-2`
           - thus `r1 + r2 = K`
       - The approach
         - create a new array which is `input[i] % k`. From the above math, it boils down to a `Pairs with sum K` problem as above

2. **Hard - 25**
## 6. Heaps - 48 [x]
  - Notes [https://en.wikipedia.org/wiki/Binary_heap]
    - Heap conceptually consists of 1 root and N child-heaps
      - **heap-property** = root is guaranteed to be the smallest(in min-heap) or largest (in max-heap) when compared to the roots of all the child-heaps.
      - recursive structure, but more effeciently defined (for adding and removing elements) as an array.
    - heap must satisfy
      - **shape-property** = complete binary tree = all levels except last one are filled.
      - **heap-property** = parent is greater(if max-heap) or lesser(if min-heap) than child nodes
    - to ensure shape-property is satisfied
      - insertion adds element to the end of the tree and then ensures heap-property is met. (up-heap. Check wikipedia link)
      - deletion swaps root for last element and then removes the last element. Then ensures heap-property is met. (down-heap. Check wikipedia link)
    - finding last element is important in insertion and deletion. Hence array is the most efficient representation of a heap
      - root at `0`
      - parent at `i`
      - left child at `2i+1`
      - right child at `2i+2`
    - to build a heap
      - follow insertion algo for each element = each insertion = `logN`. Total time = `NlogN`
      - floyd algo
        - insert elements into heap in random order without meeting heap-property.
        - from the leaf nodes, traverse upwards. At each node, verify heap property for sub-tree rooted at that element.
        - time complexity is N - [https://courses.cs.washington.edu/courses/cse373/18wi/files/slides/lecture-14-ann.pdf]


  - `PriorityQueue` is a good implementation of min-heap
  - `PriorityQueue<>(Collections.reverseOrder())` is an implemenation of max-heap.
  - Check median of running numbers - [https://www.geeksforgeeks.org/median-of-stream-of-integers-running-integers/]

## 7. Trees - 161
1. **Introduction - 17 [x]**
  - Balancing AVL - [https://www.geeksforgeeks.org/avl-tree-set-1-insertion/]
2. **Traversals - 32 [x]**
3. **Construction & Conversion - 35 [x]**
4. **Checking & Printing - 34**
5. **Summation - 33**
6. **Lowest Common Ancestor - 12**

## 8. Binary Search Trees - 60
1. **Basic**
1. **Construction and Conversion - 18**
1. **Check and Smallest/Largest Element - 42**
  - find largest element = rightmost element in a BST
  - find smallest element = leftmost element in a BST
  - in-order traversal gives elements in ascending order.
  - find the k smallest elements = in-order traversal, printing/storing k elements
  - find the k largest elements = reversal in-order traversal, printing/storing k elements

## 9. Graphs - 165
1. **Representation**
  - Adjacency Matrix = used if graph is densely connected. Rare in real-world
  - Adjacency List = More effecient, especially if implemented with hashset. Used when graph is sparse.
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
- Checkout algo description videos - [https://www.youtube.com/user/mikeysambol/videos]
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
  - Interval overlap - [http://blog.gainlo.co/index.php/2016/07/12/meeting-room-scheduling-problem/]

## 3. Greedy Algorithms - 13
## 4. Dynamic Programming - 56
  - Use `transcipt` idea to calculate time complexity - [NPTEL video][https://youtu.be/6h6Fi6AQiRM?t=2251]
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
### basics - Java
  - [https://docs.oracle.com/cd/E19455-01/806-5257/6je9h0329/index.html]
  - **Concurrency** = `n threads -> 1 processor`
  - **Parallelism** = `n threads -> m processors`
  - Threads share address-space within process
    - Cheap to create since new address-space need not be created.
    - Less time switching threads than switching processes
    - Easier to share data between threads
  - Each process has 1 or more Light Weight Processes (LWP), which are like virtual CPUs (Kernel threads)
    - Threads created in the application are User Threads
    - **Unbound user threads** -> 1 user thread bound to 1 LWP
    - **Bound user thread** -> user thread is scheduled onto any one of the available free LWPs

### Guidelines
  - [https://docs.oracle.com/cd/E19455-01/806-5257/6je9h0342/index.html]
  - Global Variables
    - global variables are not thread-safe. Use `ThreadLocal<DataType>`
      - `ThreadLocal<Integer> threadLocalValue = new ThreadLocal<>();`
      - `ThreadLocal<Integer> threadLocal = ThreadLocal.withInitial(() -> 1);`

### concurrent execution
  - [https://www.geeksforgeeks.org/different-approaches-to-concurrent-programming-in-java/]
  - parallel execution within a thread-pool
  - ```
    java.util.concurrent.Executors
    ExecutorService taskList = Executors.newFixedThreadPool(poolSize);
    taskList.execute(someRunnable)
    ```
  - Another way is to create a class that implements `Runnable` interface and pass this class instance in the constructor of `Thread` and call `thread.start()`.
  - 

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
    - can be interrupted. `Thread.sleep()` then throws and `IterruptedException`.
  - `anotherThread.join(waitTimeInMs)`
    - callingThread waits for `waitTimeinMs` for `anotherThread` to complete.
    - if no `waitTimeInMs` is specified, indefinitely wait.

## 2. Synchronization
  - basic synchronization = [https://www.geeksforgeeks.org/method-block-synchronization-java/]
  - reentrant locks = [https://www.geeksforgeeks.org/reentrant-lock-java/]
  - synchronized methods = lock on the object, when one `synchronized` method is being executed, no other `synchronized` may be executed by other threads.
    - `public synchronized int value() { return c; }`
  - synchronized block = finer granularity, gains lock on the object passed as its argument.
    - ```
      synchronized(objectToLockOn) {
        lastName = name;
        nameCount++;
      }
      ```
    - Both the above methods are reentrant.
    - Using the `java.util.concurrent.locks.Lock` interface, use `tryLock(timeoutInMs)`. It waits for `timeoutInMs` and returns `true` if it gets the lock, else `false`
    - 

## 3. Deadlocks
  - [https://www.geeksforgeeks.org/operating-system-process-management-deadlock-introduction/]

## 4. Basic socket communication - [https://www.geeksforgeeks.org/socket-programming-in-java/]
## 5. IO
  - `FileInputStream`/`FileOutputStream`
    - Read/Write in `bytes`
    - store each iteration in `int` with the last `8 bits` representing data, stop at `-1`
    - constructor takes in file-path as string
  - `FileReader`/`FileWriter`
    - Read/Write in `characters`
    - store each iteration in `int` with last `16 bits` representing data, stop at `-1`
    - constructor takes in file-path as string
  - `BufferedInputStream`/`BufferedOutputStream`
    - read and write byte streams with a buffer in-between.
    - call `flush()` method to flush the write-buffer.
    - constructor takes in `InputStream`/`OutputStream`
  - `BufferedReader`/`BufferedWriter(PrintWriter)`
    - constructor takes in `FileReader`/`FileWriter`
    - read and write character streams with a buffer in-between
    - `BufferedReader` has a `readLine()` method to read lines of text
    - `PrintWriter` has a `println(Object)`, `printf(Object)`, `format(String, Objects)` method to write data to file and flush it
  - `Scanner`
    - constructor takes in `BufferedReader`
    - read tokens separated by whitespaces
    - hasNext`datatype` and next`datatype` return datatype, `hasNext` and `next` return `String`.
  - `Console`
    - constructed using `System.console()`
    - `readLine` to read a line of data from console
    - `readPassword` to read passwords
## 6. Exceptions
  - client can recover from an exception = **checked exception**
  - client cannot recover from the exception = **unchecked exception**
## 7. Concurrency

# Concepts

## Permutation and Combination
  - safes have combinations where the order of the entered keys matters. i.e. `123 != 132`. Combinations is a misnomer, it should be permutation.
  - **permutation** = order matters => larger result set => `nPr = n! / (n-r)!`
  - **combination** = order doesnt matter => smaller result set => `nCr = nPr / r!`
  - permutations when there are repeats in the input, i.e., `1122234` => n! / for-each-char-repeated(number-of-repeats!) [https://www.mathwarehouse.com/probability/permutations-repeated-items.php]
    - `1122234` = `7! / (number-of-1s! number-of-2s! ..)` = `7! / (2! 3!)` = `(7! / 12)` = `420`
  - When asked to generate all permutations, usually **recursion/dynamic-programming** is the answer.
