---
type: template
aliases: ["Lazy Deletion", "Lazy_Deletion"]
category: core-collections
categories: [core-collections]
---

# Heap - Lazy Deletion

```java
int[] version;
boolean[] alive;
// order: 높은 점수→작은 ID
PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> {
    int order = Integer.compare(b[0], a[0]);
    return order != 0 ? order : Integer.compare(a[1], b[1]);
});
// init: ID 상태·후보 Heap 초기화
void init(int n) {
    version = new int[n];
    alive = new boolean[n];
    pq.clear();
}
// update: 버전을 올려 옛 기록과 구별하고 새 점수 삽입
void update(int id, int score) {
    version[id]++;
    alive[id] = true;
    pq.offer(new int[]{score, id, version[id]});
}
// delete: 버전 무효화·ID 비활성화
void delete(int id) {
    version[id]++;
    alive[id] = false;
}
// topId: 꼭대기의 낡은 기록을 버린 뒤 유효한 ID 조회
int topId() {
    while (!pq.isEmpty()) {
        int[] entry = pq.peek();
        int id = entry[1];
        if (alive[id] && entry[2] == version[id]) return id;
        pq.poll();
    }
    return -1;
}
```

init(n) 후 ID=0..n−1, 높은 점수→작은 ID, 없으면 −1 · 버전 int 범위 · M회 갱신 총 O(M log M), 공간 O(M+n).

**분류** [[00 Maps/Core Collections|Core Collections]]

**사용 원리** [[40 Optimization/Heap|Heap]]
