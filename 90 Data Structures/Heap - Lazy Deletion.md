---
type: template
aliases: ["Lazy Deletion", "Lazy_Deletion"]
category: core-collections
categories: [core-collections]
---

# Heap - Lazy Deletion

```text
같은 ID=2의 점수 변경: 90 → 40

Heap 기록             최신 상태
(90, id=2, v1) ──×──▶ version[2]=2
(40, id=2, v2) ──✓──▶ alive[2]=true

top 조회: 옛 90을 버림 → 최신 40만 유효
삭제: alive=false → Heap 안에 남아 있어도 무효
```

| Step | Pseudocode |
| --- | --- |
| Update | // 버전을 올리고 새 기록을 Heap에 추가한다. 옛 기록은 당장 찾지 않는다. |
| Delete | // ID를 비활성화하고 버전을 올려 기존 기록을 무효로 만든다. |
| Prune | // 꼭대기 기록이 삭제되었거나 옛 버전이면 꺼내 버린다. |
| Top | // 살아 있는 최신 기록을 만나면 ID를 반환한다. 비었으면 실패 값을 반환한다. |

## Java

```java
int[] version;
boolean[] alive;

// 기록: {점수, ID, 버전}; 높은 점수 → 작은 ID
PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> {
    int order = Integer.compare(b[0], a[0]);
    return order != 0 ? order : Integer.compare(a[1], b[1]);
});

void init(int n) {
    version = new int[n];
    alive = new boolean[n];
    pq.clear();
}

void update(int id, int score) {
    version[id]++;
    alive[id] = true;
    pq.offer(new int[]{score, id, version[id]});
}

void delete(int id) {
    version[id]++;
    alive[id] = false;
}

int topId() {
    while (!pq.isEmpty()) {
        int[] entry = pq.peek();
        int id = entry[1];
        if (alive[id] && entry[2] == version[id]) {
            return id;
        }
        pq.poll();
    }
    return -1;
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| 왜 버전도 검사? | 살아 있는 ID에도 옛 점수 기록이 남아 있다. alive만 검사하면 이미 바뀐 점수를 채택할 수 있다. |
| `peek` 후 `poll` | 유효하면 조회만 하고 남긴다. 무효한 기록만 제거한다. `topId()`는 유효한 원소를 삭제하지 않는다. |
| Comparator의 a / b | 큰 점수가 먼저라 `compare(b[0],a[0])`. 동점은 작은 ID가 먼저라 `compare(a[1],b[1])`. |
| 한 번의 조회 비용 | 옛 기록이 쌓이면 한 번에 많이 버릴 수 있다. 단, 각 기록은 한 번만 제거된다. |

`init(n)` 후 ID=0..n−1 · 없으면 −1 · 버전 int 범위 · `java.util.*` · M회 갱신에 따른 Heap 삽입·제거 총 O(M log M), 공간 O(M+n).

**분류** [[00 Maps/Core Collections|Core Collections]]

**사용 원리** [[40 Optimization/Heap|Heap]]
