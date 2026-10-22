---
type: template
aliases: ["Lazy Deletion", "Lazy_Deletion"]
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
}
void update(int id, int score) {
    version[id]++;                              // ①
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
        if (alive[id] && entry[2] == version[id]) return id; // ②
        pq.poll();
    }
    return -1;
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 갱신마다 버전 증가 | 점수가 다시 같아져도 서로 다른 기록임을 구별한다. |
| ② 현재 버전 + 살아 있음 | 옛 점수나 삭제된 원소가 답으로 나오지 않게 한다. |
| 유효한 top은 peek로 유지 | 조회만 했는데 살아 있는 원소가 사라지면 안 된다. |

**예시**

```text
ID 1 갱신: (90점,v1) → (50점,v2), 현재 version[1]=2
Heap 꼭대기 90점(v1) → 1≠2 → poll로 버림
다음 꼭대기 50점(v2) → 2=2 → 유효
topId() = 1, 유효한 기록은 Heap에 남김
```

**적용** 우선순위가 수정·삭제되는데 Heap 중간 원소를 찾기 어렵다면, 낡은 기록을 꼭대기에서 버린다.

**준비** `init(n)`을 한 번 호출한다. ID=0..n-1, 기록은 `{score,id,version}`. 높은 점수 → 작은 ID 우선.

**주의** 버전은 int가 넘치지 않는 범위. 누적 M개 삽입 시 총 O(M log M) 시간, O(M+n) 공간.

[[Heap]] · [[Hashing]] · [[Dijkstra]]
