---
type: template
tags: ["Graphs"]
aliases: ["Kuhn Algorithm"]
---

# Bipartite Matching

```java
// graph[left]는 가능한 right 목록. owner[right]=-1, seen[left]=0으로 시작.
// owner는 오른쪽 자리의 왼쪽 주인. 두 그룹은 이미 나뉘어 있어야 한다.
static boolean augment(int left, int searchId) {
    // 이번 재배치에서 같은 주인을 다시 따라가면 순환
    if (seen[left] == searchId) return false;
    seen[left] = searchId;
    for (int right : graph[left]) {
        // 빈 자리 또는 기존 주인을 옮길 수 있는 자리
        if (owner[right] == -1 || augment(owner[right], searchId)) {
            // 재배치 성공 뒤에만 바꿔 실패한 탐색은 기존 배정 유지
            owner[right] = left;
            return true;
        }
    }
    return false;
}
static int maximumMatching(int leftCount) {
    int size = 0;
    for (int left = 0; left < leftCount; left++) {
        // 새 탐색 번호로 이전에 봤던 주인도 다시 재배치 가능
        if (augment(left, left + 1)) size++;
    }
    return size;
}
```

**예시**

```text
가능: L0 → R0,R1    L1 → R0
처음  L0—R0        R1 비어 있음
추가  L1 → R0 → L0 → R1 (기존 주인을 이동)
최종  L1—R0        L0—R1 → 2개
```

**주의** Kuhn 방식. 일반 Graph 매칭에는 적용할 수 없다. O(L·E+L+R) 시간, O(L+R+E) 공간.

[[Bipartite Graph]] · [[Max Flow]] · [[Edmonds-Karp]]
