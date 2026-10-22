---
type: template
aliases: ["Lazy Deletion", "Lazy_Deletion"]
---

# Heap - Lazy Deletion

```java
// 아래 배열·Heap은 함수 밖 상태. init(n)을 한 번 호출한 뒤 ID=0..n-1을 사용한다.
int[] version;
boolean[] alive;
// 기록={score,id,version}. 높은 점수→작은 ID 우선.
PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> {
    int order = Integer.compare(b[0], a[0]);
    return order != 0 ? order : Integer.compare(a[1], b[1]);
});
void init(int n) {
    version = new int[n];
    alive = new boolean[n];
}
void update(int id, int score) {
    // 갱신마다 한 번만 증가. 점수가 다시 같아져도 옛 기록과 구별한다.
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
        // 유효한 원소는 조회 뒤에도 남겨야 하므로 poll 대신 peek.
        int[] entry = pq.peek();
        int id = entry[1];
        // 살아 있고 현재 버전인 기록만 답이다.
        if (alive[id] && entry[2] == version[id]) return id;
        // 낡은 기록은 꼭대기에 왔을 때 지운다.
        // 각 기록은 전체에서 한 번만 제거된다.
        pq.poll();
    }
    return -1; // 유효한 원소 없음. version은 int가 넘치지 않는 범위에서 사용한다.
}
```

**예시**

```text
ID 1 갱신: (90점,v1) → (50점,v2), 현재 version[1]=2
Heap 꼭대기 90점(v1) → 1≠2 → poll로 버림
다음 꼭대기 50점(v2) → 2=2 → 유효
topId() = 1, 유효한 기록은 Heap에 남김
```

**주의** 버전은 int가 넘치지 않는 범위. 누적 M개 삽입 시 총 O(M log M) 시간, O(M+n) 공간.

[[Heap]] · [[Hashing]] · [[Dijkstra]]
