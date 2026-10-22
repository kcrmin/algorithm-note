---
type: template
aliases: ["Greedy_Basics", "Interval Scheduling"]
category: greedy
categories: [greedy]
---

# Greedy

```java
int maxMeetings(int[][] meetings) {
    // order: 끝이 빠른 순서
    Arrays.sort(meetings, Comparator.comparingInt(a -> a[1]));
    int end = Integer.MIN_VALUE, answer = 0;
    for (int[] meeting : meetings) {
        // pick: 이전 끝 이상에서 시작하는 회의 선택
        if (meeting[0] < end) continue;
        answer++;
        end = meeting[1];
    }
    // answer: 선택한 회의 수
    return answer;
}
```

meetings[i]={시작,끝}, 시작<끝 · 겹치지 않는 회의 최대 개수 · 시간 O(n log n).

**분류** [[00 Maps/Greedy Choices|Greedy Choices]]
