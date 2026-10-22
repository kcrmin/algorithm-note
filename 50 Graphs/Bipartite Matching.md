---
type: template
tags: ["Graphs"]
aliases: ["Kuhn Algorithm"]
---

# Bipartite Matching

**언제** 이미 나뉜 왼쪽/오른쪽 후보 사이에서 겹치지 않는 일대일 배정을 최대화한다.

```text
가능: L0 → R0,R1    L1 → R0
처음  L0—R0        R1 비어 있음
추가  L1 → R0 → L0 → R1 (기존 주인을 이동)
최종  L1—R0        L0—R1 → 2개
```

**코드** `graph[left]`는 가능한 right 목록. `owner[right]=-1`, `seen[left]=0`으로 시작.

```java
static boolean augment(int left, int searchId) {
    if (seen[left] == searchId) return false;
    seen[left] = searchId;
    for (int right : graph[left]) {
        if (owner[right] == -1 || augment(owner[right], searchId)) { // ①
            owner[right] = left;
            return true;
        }
    }
    return false;
}
static int maximumMatching(int leftCount) {
    int size = 0;
    for (int left = 0; left < leftCount; left++) {
        if (augment(left, left + 1)) size++; // ②
    }
    return size;
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 빈 자리 또는 주인 재배치 | 빈 자리만 찾으면 L1을 배정하지 못해 최댓값을 놓친다. |
| 성공했을 때만 owner 교체 | 실패한 경로는 기존 배정을 망가뜨리면 안 된다. |
| ② 탐색마다 새 searchId | 이전 탐색에서 본 주인도 새 경로에서는 다시 옮길 수 있다. |

**주의** Kuhn 방식. 일반 Graph 매칭에는 적용할 수 없다. O(L·E+L+R) 시간, O(L+R+E) 공간.

[[Bipartite Graph]] · [[Max Flow]] · [[Edmonds-Karp]]
