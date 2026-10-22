---
type: template
aliases: ["Greedy_Basics", "Interval Scheduling"]
---

# Greedy

**언제** 지금의 선택을 확정해도 최적해가 남을 때. 아래는 겹치지 않는 회의를 최대한 많이 고른다.

```text
회의       [1,4)  [2,3)  [3,5)
끝 순 정렬 [2,3) → [1,4) → [3,5)
선택         ✓       ×       ✓
다음 시작은 이전 끝 이상: 3 ≥ 3
```

**코드** `meetings[i] = {start, end}`, start < end.

```java
Arrays.sort(meetings, Comparator.comparingInt(a -> a[1])); // ①
int end = Integer.MIN_VALUE;
int answer = 0;
for (int[] meeting : meetings) {
    if (meeting[0] < end) continue;                       // ②
    answer++;
    end = meeting[1];
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 가장 일찍 끝나는 회의 | 최적해의 첫 회의를 이것으로 바꿔도 뒤 회의와 겹치지 않는다. |
| ② 시작 < 이전 끝이면 제외 | 끝과 시작이 같으면 연달아 진행할 수 있다. |
| 선택 뒤 end 갱신 | 이후 회의는 방금 확정한 회의의 끝을 기준으로 고른다. |

**주의** 수익 최대화에는 이 기준을 그대로 쓰면 안 된다. 정렬 포함 O(n log n).

[[Sorting]] · [[DP]] · [[Heap]]
