---
type: template
tags: ["Graphs"]
aliases: ["Kuhn Algorithm"]
category: flow-matching
categories: [flow-matching]
---

# Bipartite Matching

```java
static List<Integer>[] graph;
static int[] owner, seen;

// augment: 자리를 찾으며 기존 주인 재배치
static boolean augment(int left, int searchId) {
    // skip: 이번 탐색에서 본 주인은 다시 따라가지 않기
    if (seen[left] == searchId) return false;
    seen[left] = searchId;
    for (int right : graph[left]) {
        // try: 빈 자리를 쓰거나 기존 주인을 옮기기
        if (owner[right] == -1 || augment(owner[right], searchId)) {
            // assign: 재배치 성공 뒤 현재 정점 배정
            owner[right] = left;
            return true;
        }
    }
    return false;
}
// maximumMatching: 왼쪽 정점마다 증대 탐색
static int maximumMatching(int leftCount) {
    int size = 0;
    for (int left = 0; left < leftCount; left++) {
        if (augment(left, left + 1)) size++;
    }
    return size;
}
```

`graph[left]`=가능한 right; `owner[right]=-1`, `seen[left]=0`에서 `maximumMatching(L)`; 두 그룹이 정해진 이분 그래프. O(L·E+L+R).

**분류** [[00 Maps/Flow and Matching|Flow and Matching]]

**비교** [[50 Graphs/Max Flow|Max Flow]]
