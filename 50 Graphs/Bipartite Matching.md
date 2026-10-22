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

static boolean augment(int left, int searchId) {
    if (seen[left] == searchId) return false; // 이번 재배치에서 같은 주인을 따라가는 순환 차단
    seen[left] = searchId;
    for (int right : graph[left]) {
        if (owner[right] == -1 || augment(owner[right], searchId)) {
            owner[right] = left; // 기존 주인 이동에 성공한 뒤에만 배정 변경
            return true;
        }
    }
    return false;
}
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
