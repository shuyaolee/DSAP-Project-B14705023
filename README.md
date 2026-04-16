# Smart Music Shuffle & Management System
This project targets implementing a high-efficiency music management system using a Doubly Linked List to handle dynamic playlist operations. The main focus is to design and analyze a custom shuffle algorithm that ensures unbiased randomness while maintaining a pointer-based history for seamless navigation. This serves as a practical application of memory management and algorithmic complexity discussed in the DSAP course.


## Proposal Report

### 動機與目標
<!-- 說明為什麼想做這個專題 -->
In the era of streaming services, users demand efficiency in managing large playlists. The core challenge is balancing "dynamic modification" (inserting/deleting songs) and "bi-directional navigation efficiency" after shuffling.

While many basic implementations struggle with efficient back-and-forth navigation (Previous/Next) once a playlist is randomized, this project aims to:

+ **Solve the Navigation Bottleneck**: Prove that a Doubly Linked List (DLL) is the optimal structure for *O*(1) constant-time "Previous Track" operations, whereas a Singly Linked List or Array would suffer from *O*(*n*) latency or high memory overhead in dynamic scenarios.

+ **Establish Verifiable Fairness**: Move beyond simple implementation by providing statistical proof that the shuffle algorithm achieves true, unbiased randomness.

### 預期功能
<!-- 列出預計實作的功能 -->
1. **Dynamic Playlist Management**: Real-time insertion and deletion using DLL pointers to ensure zero-shift overhead.

2. **Seamless Bi-directional Navigation**: Utilizing `prev` and `next` pointers to allow instant track switching, even in a shuffled state.

3. **Fisher-Yates Shuffle with Fairness Verificatio**: Implementing a robust randomized algorithm and conducting 1,000+ trial cycles to verify uniform distribution.

4. **Performance Benchmarking**: A built-in test mode to compare execution times between the DLL approach and traditional Array-based methods.
5. 
6. **Multimedia Integration**: Automatically triggering a browser to play YouTube URLs via system calls.

### 使用技術
<!-- 使用的語言、框架、工具等 -->
+ **Language**: C++

+ **Core Data Structures**:

  + **Doubly Linked List**: Chosen specifically for *O*(1) bi-directional traversal and efficient node re-linking during shuffle.

  + **Stack**: To track "Play History," allowing users to undo shuffles or trace back through randomized sequences.

+ **Key Algorithm**: Fisher-Yates Shuffle (optimized for pointer re-mapping).
+ 
+ **System Tools**: `stdlib.h` (`system()` function) for URL handling.


### 時程規劃
<!-- 各週預計完成的進度 -->
+ **Week 7-8**: Define `struct Node` and implement basic DLL operations (Insert, Delete, Display).

+ **Week 9-11**: Develop the Shuffle logic and the Statistical Testing Module to verify randomness.

+ **Week 12-13**: Implement YouTube URL triggering and refine the CLI.

+ **Week 14-15**: Compare DLL vs. Array performance with 1,000+ nodes and finalize the report.

### 與課程的關聯
<!-- 你的專題可能涉及哪些資料結構或演算法概念？為什麼？ -->
This project applies several fundamental concepts from the DSAP course:

1. **Trade-off Analysis**: Deep dive into why DLL is superior to Arrays for high-frequency dynamic updates in music playlists.

2. **Pointer Integrity**: Managing complex pointer re-mapping during the shuffle process to avoid memory leaks or dangling pointers.

3. **Empirical Complexity Analysis**: Moving from theoretical Big-O notation to practical execution time measurement and data-driven verification.

---

## Prototype Report

### 目前進度
<!-- 完成了什麼 -->

### 遇到的困難
<!-- 遇到什麼問題、如何解決或打算如何解決 -->

### 下一步計畫
<!-- 接下來要做什麼 -->

### 與課程的關聯
<!-- 到目前為止，你的實作中哪些部分與課程內容有關？關係是什麼？ -->

---

## Final Report

### 專案說明
<!-- 完整描述你的專案做了什麼 -->

### 使用方式
<!-- 如何編譯、執行、使用你的程式 -->

### 與課程的關聯總結
<!-- 總結你的專題與進階程式設計及資料結構課程之間的關聯 -->
