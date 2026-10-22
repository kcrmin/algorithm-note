---
type: template
aliases: ["Greedy_Basics", "Interval Scheduling"]
category: greedy
categories: [greedy]
---

# Greedy

```java
int maxMeetings(int[][] meetings) {
    // 끝이 빠른 회의로 바꿔도 뒤 회의를 방해하지 않는다.
    Arrays.sort(meetings, Comparator.comparingInt(a -> a[1]));
    int end = Integer.MIN_VALUE, answer = 0;
    for (int[] meeting : meetings) {
        if (meeting[0] < end) continue; // 시작=이전 끝이면 이어서 가능하다.
        answer++;
        end = meeting[1];
    }
    return answer;
}
```

meetings[i]={시작,끝}, 시작<끝 · 겹치지 않는 회의 최대 개수 · 시간 O(n log n).

**분류** [[00 Maps/Greedy Choices|Greedy Choices]]
