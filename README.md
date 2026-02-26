# algo-docs

알고리즘·자료구조 노트 (한 줄 요약 → Core Concept → API → Patterns → Pitfalls). Java 코드 중심.

## 구조

```
algo-docs/
├── 01_Foundations/
│   ├── Techniques/
│   │   ├── Bitmask.md
│   │   ├── Prefix_Sum.md
│   │   ├── Sliding_Window.md
│   │   ├── Two_Pointers.md
│   │   ├── Coordinate_Compression.md
│   │   ├── Visited.md
│   │   └── Distance.md
│   ├── Data_Structures/
│   │   ├── Priority_Queue.md
│   │   ├── Union_Find.md
│   │   ├── Segment_Tree.md
│   │   └── Fenwick_Tree.md
│   ├── Strings/
│   │   ├── KMP.md
│   │   └── Trie.md
│   └── Math/
│       ├── GCD_LCM.md
│       └── Modular_Arithmetic.md
│
└── 02_Algorithm/
    ├── Search/
    │   └── Binary_Search.md
    ├── Graph/
    │   ├── Basics/
    │   │   ├── Graph_Basics.md
    │   │   ├── Undirected_Graph.md
    │   │   ├── Directed_Graph.md
    │   │   ├── Weighted_Graph.md
    │   │   └── Bipartite_Graph.md
    │   ├── Tree/
    │   │   └── Tree.md
    │   ├── Topological_Search.md
    │   ├── MST.md
    │   ├── SCC.md
    │   └── Shortest_Path/
    │       ├── Dijkstra.md
    │       ├── Bellman_Ford.md
    │       └── Floyd_Warshall.md
    └── Optimization/
        ├── Greedy/
        │   └── Greedy_Basics.md
        ├── DP/
        │   ├── DP_Basics.md
        │   ├── Knapsack.md
        │   └── LIS.md
        └── Backtracking/
            ├── Backtracking.md
            ├── Pruning.md
            ├── Memoization.md
            ├── Combinations_DFS.md
            └── Permutations_DFS.md
```

## 문서 형식

- **한 줄 요약**: 주제 한 줄 정의
- **Core Concept**: When to use, Key Point
- **API & State Change**: 연산/상태 표
- **Patterns**: 코드 패턴 (A, B, …)
- **Pitfalls & Tips**: 자주 하는 실수

새 문서는 `template.md`를 복사해 사용하면 됨.
