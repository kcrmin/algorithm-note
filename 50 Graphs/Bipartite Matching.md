---
type: template
tags: ["Graphs"]
aliases: ["Kuhn Algorithm"]
category: flow-matching
categories: [flow-matching]
---

# Bipartite Matching

```text
가능한 짝: A → {1,2}, B → {1}

처음:  A — 1    B는 빈자리가 없음
재배치: B → 1 → A → 2
결과:  A — 2    B — 1

빈자리까지 연결되면 기존 짝을 바꾸며 한 쌍 증가
```

| Step | Pseudocode |
|---|---|
| Try | // 왼쪽 정점 하나가 들어갈 오른쪽 자리를 찾는다. |
| Reroute | // 자리에 주인이 있으면 그 주인이 다른 자리로 옮길 수 있는지 찾는다. |
| Assign | // 빈자리를 찾거나 재배치에 성공했을 때만 새 주인을 기록한다. |
| Count | // 증대 탐색에 성공할 때마다 매칭 크기를 하나 늘린다. |

## Java

```java
static List<Integer>[] graph;
static int[] owner, seen;

static boolean augment(int left, int searchId) {
    if (seen[left] == searchId) {
        return false;
    }

    seen[left] = searchId;

    for (int right : graph[left]) {
        if (owner[right] == -1 || augment(owner[right], searchId)) {
            // 빈자리 또는 기존 주인의 이동이 확보된 뒤 배정.
            owner[right] = left;
            return true;
        }
    }

    return false;
}

static int maximumMatching(int leftCount) {
    int size = 0;

    for (int left = 0; left < leftCount; left++) {
        if (augment(left, left + 1)) {
            size++;
        }
    }

    return size;
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| `owner[right] = left` | 오른쪽 자리의 현재 주인을 저장한다. 오른쪽 정점은 한 명만 받을 수 있다. |
| 탐색마다 새 방문 표시 | 한 탐색 안에서 같은 왼쪽 정점을 재방문하면 순환한다. 다음 출발에서는 다시 탐색할 수 있어야 한다. |
| 빈자리 또는 재배치 성공 | 기존 주인을 먼저 옮길 수 있어야 현재 정점을 배정할 수 있다. |
| 초기화 | owner=-1, seen=0. searchId는 1부터 시작해 미방문 0과 구분한다. |

`graph[left]`는 가능한 right 목록. 두 그룹이 정해진 이분 그래프에서 `maximumMatching(L)`. **O(L·E+L+R)**.

**분류** [[00 Maps/Flow and Matching|Flow and Matching]]

**비교** [[50 Graphs/Max Flow|Max Flow]]
