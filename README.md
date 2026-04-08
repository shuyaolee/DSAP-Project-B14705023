# Smart Music Shuffle & Management System
This project targets implementing a high-efficiency music management system using a Doubly Linked List to handle dynamic playlist operations. The main focus is to design and analyze a custom shuffle algorithm that ensures unbiased randomness while maintaining a pointer-based history for seamless navigation. This serves as a practical application of memory management and algorithmic complexity discussed in the DSAP course.


## Proposal Report

### 動機與目標
<!-- 說明為什麼想做這個專題 -->
In the era of streaming services, users demand efficiency in managing large playlists. The core challenge is balancing "dynamic modification" (inserting/deleting songs) and "fair randomness" (shuffling without repetition).

This project aims to simulate the underlying logic of a music player. By focusing on data structure efficiency rather than a complex GUI, we will explore how to manage memory and pointers to achieve *O*(1) time complexity for list operations and implement a robust shuffle algorithm.

### 預期功能
<!-- 列出預計實作的功能 -->
1. **Dynamic Playlist Management**: Support for loading song data from external files, with real-time "insert at" (queueing) and deletion functions.

2. **Bi-directional Navigation**: Implementing "Next" and "Previous" features using a pointer-based approach to ensure seamless navigation even in shuffle mode.

3. **Fisher-Yates Shuffle Mechanism**: A randomized algorithm to ensure the playlist is shuffled uniformly, avoiding the common "repetitive song" issue in simple random sampling.

4. **Multimedia Integration**: Automatically triggering a browser to play the corresponding YouTube URL when a song is selected via system calls.

### 使用技術
<!-- 使用的語言、框架、工具等 -->
+ **Language**: C++

+ **Core Data Structures**:

  + **Doubly Linked List**: To store the playlist, ensuring efficient node insertion and removal.

  + **Stack**: To store "Play History," enabling the "Previous Song" functionality in shuffle mode.

+ **Key Algorithm**: Fisher-Yates Shuffle (for re-linking pointer sequences).

+ **System Tools**: `stdlib.h` (`system()` function) for URL handling.

### 時程規劃
<!-- 各週預計完成的進度 -->
+ **Week 7-8**: Define `struct Node` and implement basic Doubly Linked List operations (Insert, Delete, Display).

+ **Week 9-11**: Develop the Shuffle logic and integrate the Stack-based history tracker.

+ **Week 12-13**: Implement the YouTube URL triggering and optimize the Command Line Interface.

+ **Week 14-15**: Conduct performance testing (e.g., response time for 1,000+ songs) and finalize the report.

### 與課程的關聯
<!-- 你的專題可能涉及哪些資料結構或演算法概念？為什麼？ -->
This project applies several fundamental concepts from the DSAP course:

1. **Selection of Data Structures**: Demonstrating why Linked Lists outperform Arrays in scenarios with frequent insertions/deletions.

2. **Pointer Manipulation**: The shuffle mechanism relies on re-mapping node pointers, requiring a deep understanding of memory addresses.

3. **Complexity Analysis**: We will analyze the time complexity of different operations to bridge the gap between theoretical DSAP principles and practical application.

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
