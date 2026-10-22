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
    version[id]++; // 점수가 다시 같아져도 옛 기록과 구별한다.
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
        if (alive[id] && entry[2] == version[id]) return id;
        pq.poll(); // 낡은 기록은 꼭대기에 왔을 때 한 번만 지운다.
    }
    return -1;
}
```

init(n) 후 ID=0..n−1, 높은 점수→작은 ID, 없으면 −1 · 버전 int 범위 · M회 갱신 총 O(M log M), 공간 O(M+n).

**분류** [[00 Maps/Core Collections|Core Collections]]

**사용 원리** [[40 Optimization/Heap|Heap]]
