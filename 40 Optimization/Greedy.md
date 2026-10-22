---
type: template
aliases: ["Greedy_Basics", "Interval Scheduling"]
---

# Greedy

```java
// meetings[i]={시작,끝}, 시작<끝. 겹치지 않는 회의의 최대 개수.
// 일찍 끝나는 회의로 바꿔도 뒤 회의를 방해하지 않는다.
Arrays.sort(meetings, Comparator.comparingInt(a -> a[1]));
int end = Integer.MIN_VALUE; // 첫 회의는 시작 시각과 관계없이 선택 가능하다.
int answer = 0;
for (int[] meeting : meetings) {
    // 이전 끝과 시작이 같으면 연달아 진행할 수 있다.
    if (meeting[0] < end) continue;
    answer++;
    end = meeting[1]; // 방금 확정한 회의의 끝이 다음 선택의 기준이다.
}
```

**예시**

```text
회의       [1,4)  [2,3)  [3,5)
끝 순 정렬 [2,3) → [1,4) → [3,5)
선택         ✓       ×       ✓
다음 시작은 이전 끝 이상: 3 ≥ 3
```

**주의** 수익 최대화에는 이 기준을 그대로 쓰면 안 된다. 정렬 포함 O(n log n).

[[Sorting]] · [[DP]] · [[Heap]]
