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
+ **Multi-Modal Zero-Pruning Graph Construction**: Successfully implemented the Adjacency List using C++ `std::unordered_map` and `std::vector` to build the "Taipei Micro-Universe" transit network. The system losslessly integrates multi-modal edges (MRT lines including Bannan, Tamsui-Xinyi, and Songshan-Xindian with the Xiaonanmen transfer, direct/transfer buses, YouBike, and walking), achieving an optimal $O(V+E)$ space complexity.

+ **Dynamic Multi-Strategy Traversal Engine**: Completed the core Min-Heap Dijkstra algorithm and successfully implemented three dynamic routing modes:
  
    + **Shortest Time Mode**: Utilizes pure physical transit time as the primary weight for maximum-speed routing.
 
    + **Minimum Transfers Mode**: Dynamically injects a "Transfer Penalty" during traversal via string matching, successfully guiding the algorithm to avoid heavily fragmented, multi-transfer routes.

    + **Lowest Cost Mode**: Overcame Dijkstra's linear additive limitation by adding a continuous station counter to the state node (`PQNode`), perfectly executing the MRT's "zonal step-fare logic."

+ **Path Compression & Backtracking**: Successfully reverse-engineered routes from the destination using a Parent-Pointer tracking mechanism. To optimize User Experience (UX), a path compression algorithm was implemented to collapse continuous rides on the same transit line into a single transit segment (e.g., compressing multiple sequential MRT stations into a clean `[Ximen] --> (MRT_Songshan_Xindian) --> [Gongguan]` output).

+ **Synthetic Stress-Testing Harness**: Developed an automated graph generator (`generateSyntheticGraph`) capable of instantly weaving a massive virtual metropolitan network in memory containing 10,000 nodes and 50,000 edges, providing a rigorous benchmark environment for the algorithms.

### 遇到的困難
<!-- 遇到什麼問題、如何解決或打算如何解決 -->
+ **Path-Dependent Weight Constraints (Non-Linear Pricing)**: Real-world MRT fares utilize zonal step-pricing (e.g., a flat base rate for initial stations, with marginal increases only when crossing zones). This violates standard Dijkstra's assumption that edge weights must be independent and linearly additive, which initially caused calculated fares to inflate inaccurately.
    + **Solution**: Implemented a "State-Space Search" expansion. Upgraded the `PQNode` to track the "continuous MRT stations traveled" (`mrtStationCount`) and the "previous transit mode." During the relaxation phase, the algorithm dynamically applies modulo logic based on the node's state to determine the exact financial cost of the edge, successfully resolving the conflict without altering the static graph structure.

+ **Redundant Traversal Node Spam on CLI**: In the basic route backtracking, because MRT stations are connected sequentially, the system indiscriminately printed every intermediate station. This made direct routes look extremely verbose and difficult to read on the Command Line Interface.
    + **Solution**: Designed a buffer-and-collapse mechanism. During route backtracking, nodes are collected in reverse order, and adjacent edges are evaluated using a loop on their `transitMode`. The CLI output is now only triggered when a mode switch (transfer) occurs or the destination is reached, successfully achieving a path compression effect identical to commercial navigation apps.

### 下一步計畫
<!-- 接下來要做什麼 -->
+ **Performance Curve Exporting & Multi-Sample Benchmarking**: We have successfully completed a single-run stress test on a 10,000-node scale, validating the nearly 200x performance gap between the `Unsorted Array` (17,561 ms) and the `Min-Heap` (89 ms). The next phase is to write an automated testing loop that incrementally tests graph loads from 1,000 to 20,000 nodes, exporting the CPU execution time data as a `.csv` file. This will be used to plot the actual time complexity curves of $O(V^2)$ vs. $O((V+E)logV)$ for the final report.
  
+ **File I/O Configuration Parser**: To completely decouple the transit data from the program logic, the next step is to replace the hardcoded `buildMicroUniverse()` function. We will utilize standard C++ File I/O to read external `.txt` or `.csv` configuration files, allowing users to freely define and expand the city's transit nodes and travel parameters without recompiling the code.

+ **Dynamic Edge Disruption Simulation**: To further challenge the limits of commercial maps, we plan to introduce a "dynamic network disruption" feature. During routing, the system will randomly set the weights of specific edges (e.g., a suspended MRT segment or an empty YouBike station) to infinity (`INF`). This will verify the engine's resiliency and real-time rerouting capabilities when facing unexpected urban traffic events.

---

## Final Report

### 專案說明
<!-- 完整描述你的專案做了什麼 -->

### 使用方式
<!-- 如何編譯、執行、使用你的程式 -->

