# [Taipei Urban Hidden Router: Multimodal Transit Engine based on Dijkstra's Algorithm]

## Proposal Report

### 動機與目標
<!-- 說明為什麼想做這個專題 -->
In densely populated urban environments like Taipei City, commuters heavily rely on commercial navigation applications to manage complex, cross-modal transit routes (e.g., combining MRT, buses, YouBike, and walking). The core challenge in modern routing is balancing "server computing efficiency" and "route comprehensiveness". While other applications on the market highlight the former feature, we would like to try to focus on the latter one.

This project aims to propose an optimized implementation of a multimodal transit routing engine that challenges the limitations of commercial applications. By focusing on graph traversal efficiency and data structure optimization rather than a complex GUI, this project demonstrates how an uncompromised routing approach can efficiently uncover the "hidden optimal routes" that commercial apps prematurely discard due to aggressive heuristic pruning.

Our goals are:

+ **Solve the Heuristic Pruning Bottleneck**: Prove that an uncompromised, pure graph traversal can discover highly efficient multi-modal combinations (e.g., walking to a YouBike station to catch a specific bus) that commercial heuristic algorithms (like A*) often filter out to save memory.

+ **Enable Dynamic Weight Adaptation**: Move beyond static routing by allowing the system to instantly rebuild graph traversal logic based on user-defined priorities (Time-Optimized, Cost-Optimized and Minimum-Transfers), proving the flexibility of adjacency lists.

+ **Comparative Performance Analysis**: Implement a specific algorithmic flow—"Minimum-Distance Node Extraction in Dijkstra's Algorithm"—using two different data structures (Min-Heap / Priority Queue vs. Unsorted Array). We will conduct a practical performance analysis to evaluate their execution time and CPU overhead under massive graph loads.


### 競品比較
<!-- 比較目前已經存在可取得的類似工具或應用 -->
While commercial systems must employ heuristic pruning (like A*) to achieve sub-second response times for millions of global users concurrently, this compromises hyper-local accuracy. Our system deliberately trades global scalability for absolute local precision. By bounding the scope to a specific metropolitan area (Taipei City) and assuming local client-side execution, we should be able to prove that uncompromised Dijkstra's algorithm is entirely computationally feasible, thereby unlocking the deep micro-transit combinations that cloud-based services must discard.


| Feature  | Commercial Apps | This System |
| :-------------: | ------------- | ------------- |
| **Routing Algorithm** | Heuristic / Pruned (A*, sacrifices hidden routes to save server load) | Uncompromised Pure Dijkstra (Guarantees the mathematical global optimum) |
| **Cross-Modal Integration** | Siloed / Limited combinations (Usually restricts to Walk + Transit) | Deep Micro-Transit Fusion (Seamlessly evaluates YouBike + Bus + Walk simultaneously) |
| **Micro-Edge Processing** | Skips hyper-local nodes (e.g., minor bike stations) for speed | Evaluates all micro-edges for extreme accuracy and hidden route discovery |


### 預期功能
<!-- 列出預計實作的功能 -->
1. **Zero-Pruning Adjacency Graph Construction**: Real-time instantiation of a unified city transit network. By utilizing an Adjacency List structure, the system achieves $O(V+E)$ memory efficiency. This specific architecture allows the engine to losslessly store hyper-local nodes (e.g., specific YouBike docks and walking alleys) without heuristic pruning, ensuring a complete and uncompromised dataset for the routing algorithm.

2. **Dynamic Weight & Penalty Adaptation**: Enables instant path recalculation based on user-defined constraints ("Shortest Time", "Lowest Cost", "Minimum Transfers"). Instead of rebuilding the graph, the engine dynamically evaluates multi-dimensional edge weights and actively injects algorithmic penalties (e.g., adding an artificial +15 point penalty when the algorithm detects a mode-switch from MRT to Bus) during traversal.

3. **Cross-Modal Path Backtracking**: Moves beyond simply calculating the final distance by implementing a robust parent-pointer (or previous-node) tracking mechanism during the Dijkstra traversal. This allows the system to reverse-engineer and output precise, step-by-step navigation instructions (e.g., Walk → YouBike → MRT) representing the absolute mathematical optimum.

4. **Synthetic Stress-Testing & Benchmarking**: Features a built-in automated data generator capable of spawning a massive virtual network (e.g., 10,000+ nodes and 50,000+ edges). This strictly powers a rigorous, millisecond-level CPU execution comparison between the $O(logV)$ Min-Heap and the baseline $O(V)$ Unsorted Array implementations during the minimum-distance node extraction phase.


### 使用技術
<!-- 使用的語言、框架、工具等 -->
+ **Language**: Standard C++

+ **Core Data Structures**:

  + **Adjacency List (Graph)**: Implemented via `std::vector` to ensure $O(V+E)$ memory efficiency, which is mathematically optimal for highly sparse urban transit networks compared to an $O(V^2)$ Adjacency Matrix.

  + **Min-Heap (Priority Queue)**: Implemented to optimize the greedy node-extraction phase in Dijkstra's algorithm, drastically reducing the update complexity from $O(V)$ to $O(logV)$.

  + **Hash Map (`std::unordered_map`)**: Utilized for $O(1)$ string-to-node indexing (mapping real-world station names to graph logic) and maintaining the parent-pointer tracking system for route backtracking.
 
  + **Unsorted Array**: Utilized strictly as a baseline control structure for the algorithm performance benchmark.

+ **Key Algorithm**: Pure Dijkstra's Shortest Path Algorithm (augmented with custom state-comparators and dynamic penalty injection).
  
+ **System Tools**: C++ Standard Template Library (STL), specifically `<chrono>` for high-precision microsecond-level benchmarking, alongside `<vector>`, `<queue>`, and `<unordered_map>`.


### Prototype 預計可驗證內容
By the Prototype phase, the following core functionalities will be fully testable via the Command Line Interface (CLI):

1. **Static Micro-Network Initialization**: Successfully instantiate the C++ Adjacency List using a hardcoded mock dataset representing a "Taipei Micro-Universe" (15-20 real-world hubs including specific MRT stations, bus stops, and YouBike docks) to serve as the baseline for functional verification.

2. **Cross-Modal Route Extraction**: Users can input a starting node and destination via the CLI. The system will utilize the parent-pointer mechanism to output not just the total minimal weight, but the exact step-by-step transit instructions (e.g., `[Start] Ximen -> (Walk 3m) -> Plant Garden -> (YouBike 12m) -> Gongguan [End]`).

3. **Dynamic Penalty Verification**: A working command to toggle the algorithmic constraints between "Shortest Time" and "Minimum Transfers". The CLI will visibly demonstrate that injecting the cross-modal transfer penalty successfully forces Dijkstra's algorithm to abandon a heavily fragmented fast route in favor of a direct, low-transfer alternative.

4. **Algorithmic Performance Benchmark**: Execution of the `generateSyntheticGraph()` module to instantly build a massive stress-test network (e.g., 10,000 virtual nodes and 50,000 edges). The prototype will utilize `<chrono>` to output the exact execution time (in milliseconds) of the shortest-path calculation, providing a strict performance comparison between the $O(logV)$ Min-Heap and the $O(V)$ Unsorted Array node-extraction implementations.


---

## Prototype Report

### 目前進度
<!-- 完成了什麼 -->
+ **Graph Data Structure Implementation**: Successfully defined the `Edge` and `Station` structures. Implemented the Adjacency List using `std::unordered_map` and `std::vector`, ensuring optimal *O*(*V*+*E*) memory utilization, which is crucial for representing sparse urban transit networks.

+ **Multi-Modal Edge Definition**: Engineered the Edge struct to hold multiple weight parameters (`timeCost`, `financialCost`) and `transitMode` (e.g., Walk, MRT, YouBike). This allows the graph to support dynamic weight shifting based on user preference.

+ **Core Routing Engine (Min-Heap Dijkstra)**: Implemented the primary shortest-path algorithm utilizing C++'s `std::priority_queue` (acting as a Min-Heap). The engine can successfully traverse the adjacency list and extract the optimal route based on the selected weight parameter in *O*((*V*+*E*)*logV*) time.

### 遇到的困難
<!-- 遇到什麼問題、如何解決或打算如何解決 -->
+ **The `decrease-key` Limitation in C++ Priority Queue**: Standard C++ `std::priority_queue` does not support a direct `decrease-key` operation to update node weights dynamically during Dijkstra's traversal.
    + **Solution**: Adopted the "Lazy Deletion" (or Visited Set) approach. Instead of updating existing nodes in the heap, the system pushes duplicate nodes with smaller distances and utilizes a boolean `visited` map to simply ignore outdated, higher-distance nodes when they are popped.

+ **Dynamic Weight Re-evaluation**: Implemented a state flag (`isTimeOptimized`) that is passed into the priority queue's custom comparator. This allows the same graph in memory to be evaluated differently in real-time without duplicating data.
    + **Solution**: Manually enforced `head->prev = nullptr` and `tail->next = nullptr` at the end of the shuffle function to ensure the structural integrity of the Doubly Linked List.

### 下一步計畫
<!-- 接下來要做什麼 -->
+ **Baseline Performance Benchmarking (Array implementation)**: The next critical phase is writing the "Unsorted Array" version of the node-extraction phase. I will integrate the `<chrono>` library to record the CPU execution time (in milliseconds) to formally compare the Array vs. Min-Heap performance.

+ **Synthetic Graph Generator**: Develop a data generation loop to randomly create a massive stress-test network consisting of 10,000 virtual stations and 50,000 interconnecting edges for the performance benchmark.

+ **Path Tracing & CLI Development**: Currently, the engine calculates the shortest distance but does not print the step-by-step route. I will implement a `previous_node` tracking map to allow backtracking and output a clear, user-friendly navigation instruction set via the Command Line Interface.


---

## Final Report

### 專案說明
<!-- 完整描述你的專案做了什麼 -->

### 使用方式
<!-- 如何編譯、執行、使用你的程式 -->

