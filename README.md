# High-Efficiency Music Engine with Fairness Verification

## Proposal Report

### 動機與目標
<!-- 說明為什麼想做這個專題 -->

In the era of streaming services, users demand efficiency in managing large playlists. The core challenge is balancing "dynamic modification" (inserting/deleting songs) and "bi-directional navigation efficiency" after shuffling.

This project aims to simulate the underlying logic of a music player. By focusing on data structure efficiency rather than a complex GUI, I will explore how to manage memory and pointers to achieve O(1) time complexity for list operations and implement a robust shuffle algorithm. While many basic implementations struggle with efficient back-and-forth navigation (Previous/Next) once a playlist is randomized, this project aims to:

+ **Performance Analysis Comparison**: Implement a specific functional flow—"Random Shuffling and Instant Queue Insertion"—using two different data structures (Doubly Linked List vs. Array). We will conduct a practical performance analysis to evaluate their execution time and memory efficiency.

+ **Solve the Navigation Bottleneck**: Prove that a Doubly Linked List (DLL) is the optimal structure for *O*(1) constant-time "Previous Track" operations, whereas a Singly Linked List or Array would suffer from *O*(*n*) latency or high memory overhead in dynamic scenarios.

+ **Establish Verifiable Fairness**: Move beyond simple implementation by providing statistical proof that the shuffle algorithm achieves true, unbiased randomness.


### 競品比較
<!-- 比較目前已經存在可取得的類似工具或應用 -->

Commercial music streaming apps (e.g., Spotify, Apple Music) offer comprehensive features but often consume significant system resources and utilize opaque shuffle algorithms.


| Feature  | Commercial Apps | This System |
| :-------------: | ------------- | ------------- |
| **Resource Usage** | High (Heavy GUI, background tracking)  | Ultra-Low (CLI-based, pointer-level optimization)  |
| **Shuffle Logic** | Opaque / Weighted (often feels repetitive)  | Transparent (True Fisher-Yates with statistical verification)  |
| **Archutecture Transparency** | Black-box (User cannot see the quene logic)  | Data-Driven (Outputs visual performance metrics of DLL vs Array)  |


### 預期功能
<!-- 列出預計實作的功能 -->
1. **Dynamic Playlist Management**: Real-time insertion and deletion using DLL pointers to ensure zero-shift overhead.

2. **Seamless Bi-directional Navigation**: Utilizing `prev` and `next` pointers to allow instant track switching, even in a shuffled state.

3. **Fisher-Yates Shuffle with Fairness Verificatio**: Implementing a robust randomized algorithm and conducting 1,000+ trial cycles to verify uniform distribution.

4. **Performance Benchmarking**: A built-in test mode to compare execution times between the DLL approach and traditional Array-based methods.

5. **Multimedia Integration**: Automatically triggering a browser to play YouTube URLs via system calls.


### 使用技術
<!-- 使用的語言、框架、工具等 -->
+ **Language**: C++

+ **Core Data Structures**:

  + **Doubly Linked List**: Chosen specifically for *O*(1) bi-directional traversal and efficient node re-linking during shuffle.

  + **Stack**: To track "Play History," allowing users to undo shuffles or trace back through randomized sequences.

+ **Key Algorithm**: Fisher-Yates Shuffle (optimized for pointer re-mapping).
  
+ **System Tools**: `stdlib.h` (`system()` function) for URL handling.


### Prototype 預計可驗證內容

By the Prototype submission phase, the following core functionalities will be testable via the Command Line Interface (CLI):

1. **Data Loading & Construction**: Successfully read a `.txt` file containing 100+ songs (Title + URL) and construct both the Doubly Linked List and Array.

2. **Basic Navigation**: Users can input commands to trigger "Play Next" and "Play Previous," traversing the pointers correctly.

3. **The "Shuffle" Execution**: working command to execute the Fisher-Yates shuffle, displaying the playlist before and after the shuffle to verify pointer re-assignment.

4. **Initial Performance Metric**: A built-in timer mechanism that outputs the microsecond execution time when a user inserts a new song into the list, providing the first set of comparative data between DLL and Array.


---

## Prototype Report

### 目前進度
<!-- 完成了什麼 -->


### 遇到的困難
<!-- 遇到什麼問題、如何解決或打算如何解決 -->

### 下一步計畫
<!-- 接下來要做什麼 -->

---

## Final Report

### 專案說明
<!-- 完整描述你的專案做了什麼 -->

### 使用方式
<!-- 如何編譯、執行、使用你的程式 -->

