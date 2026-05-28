# [Urban Hidden Router: Multimodal Transit Engine based on Dijkstra's Algorithm]

## Proposal Report

### 動機與目標
<!-- 說明為什麼想做這個專題 -->
In densely populated urban environments, commuters heavily rely on commercial navigation applications to manage complex, cross-modal transit routes (e.g., combining MRT, buses, YouBike, and walking). The core challenge in modern routing is balancing "server computing efficiency" and "route comprehensiveness."

This project aims to simulate the underlying logic of a multimodal transit routing engine. By focusing on graph traversal efficiency rather than a complex GUI, I will explore how to manage vertices and edges to uncover "hidden optimal routes" that commercial apps often skip due to heuristic pruning. While major mapping services struggle to provide deep, highly fragmented transit combinations, this project aims to:

+ **Compare Performance Analysis**: Implement a specific algorithmic flow—"Minimum-Distance Node Extraction in Dijkstra's Algorithm"—using two different data structures (Min-Heap / Priority Queue vs. Unsorted Array). We will conduct a practical performance analysis to evaluate their execution time and CPU overhead under massive graph loads.

+ **Solve the Heuristic Pruning Bottleneck**: Prove that an uncompromised, pure graph traversal can discover highly efficient multi-modal combinations (e.g., walking to a YouBike station to catch a specific bus) that commercial heuristic algorithms (like A*) often filter out to save memory.

+ **Establish Dynamic Weight Adaptations**: Move beyond static routing by allowing the system to instantly rebuild graph traversal logic based on user-defined priorities (Time-Optimized vs. Cost-Optimized), proving the flexibility of adjacency lists.


### 競品比較
<!-- 比較目前已經存在可取得的類似工具或應用 -->
Commercial transit apps (e.g., Google Maps, Apple Maps) offer comprehensive geographical features but often utilize opaque, pruned routing algorithms that sacrifice optimal micro-transit combinations for faster server response times.


| Feature  | Commercial Apps | This System |
| :-------------: | ------------- | ------------- |
| **Routing Algorithm** | Heuristic / Pruned (A*, skips minor nodes) | Uncompromised Pure Dijkstra's Algorithm |
| **Edge Processing** | Often restricted to 1 or 2 transit modes | Comprehensive Multi-Modal (Walk + Bike + Transit) |
| **Architecture Transparency** | Black-box (User cannot see the traversal logic) | Data-Driven (Outputs visual performance metrics of Min-Heap vs Array) |


### 預期功能
<!-- 列出預計實作的功能 -->
1. **Multi-Modal Graph Construction**: Real-time construction of a city transit network using an Adjacency List to ensure *O*(*V*+*E*) memory efficiency for sparse networks.

2. **Dynamic Weight Shifting**: Utilizing dual-weight edges to allow instant path recalculation when a user switches between "Shortest Time" and "Lowest Cost" modes.

3. **Deep Route Extraction**: Outputting clear, step-by-step cross-platform transit instructions, demonstrating the successful traversal of the graph.

4. **Performance Benchmarking**: A built-in stress test mode to compare execution times and time complexity between the Min-Heap approach and traditional Array-based methods during node extraction.


### 使用技術
<!-- 使用的語言、框架、工具等 -->
+ **Language**: C++

+ **Core Data Structures**:

  + **Adjacency List (Graph)**: Chosen specifically for efficient memory utilization in sparse urban networks compared to Adjacency Matrices.

  + **Min-Heap (Priority Queue)**: Implemented to optimize the node extraction phase in Dijkstra's algorithm, achieving *O*(*logV*) updates instead of *O*(*V*).

  + **Unsorted Array**: Utilized strictly as a baseline control structure for the performance benchmark.

+ **Key Algorithm**: Dijkstra's Shortest Path Algorithm (optimized for dynamic weight re-mapping).
  
+ **System Tools**: Standard C++ libraries (`<chrono>` for microsecond-level benchmarking, `<vector>`, `<queue>`).


### Prototype 預計可驗證內容
By the Prototype submission phase, the following core functionalities will be testable via the Command Line Interface (CLI):

1. **Graph Construction & Data Loading**: Successfully construct the Adjacency List representing a micro-network of 20+ known Taipei transit hubs and interconnected routes.

2. **Basic Shortest Path Execution**: Users can input a starting node and destination node, and the system will traverse the graph to output the optimal multi-modal route.

3. **Priority Mode Switching**: A working command to switch the graph's weight parameters (Time vs. Cost) and output the correspondingly altered route to verify dynamic edge evaluation.

4. **Initial Performance Metric**: A built-in timer mechanism that automatically generates a synthetic graph of 5,000+ nodes, outputting the microsecond execution time of the shortest-path calculation, providing the first set of comparative data between Min-Heap and Array.


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

