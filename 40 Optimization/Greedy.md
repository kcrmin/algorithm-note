---
type: template
aliases: ["Greedy_Basics", "Interval Scheduling"]
category: greedy
categories: [greedy]
---

# Greedy

```text
겹치지 않는 회의를 최대한 많이 선택 — 종료 시각순으로 보기

time    1   2   3   4   5   6
A       [-------)                 ✓
C               [---)             ✓
B           [-----------)         ×  앞 회의와 겹침
D                   [-------)     ✓

A → C → D = 3개
매번 가장 일찍 끝내면, 뒤에 쓸 시간이 가장 많이 남는다.
```

| Step | Pseudocode |
| --- | --- |
| Order | // 끝나는 시각이 빠른 회의부터 살펴본다 |
| Skip | // 이미 고른 회의가 끝나기 전에 시작하면 건너뛴다 |
| Pick | // 겹치지 않으면 선택하고, 마지막 종료 시각을 바꾼다 |
| Answer | // 선택한 회의 수를 반환한다 |

## Java

```java
int maxMeetings(int[][] meetings) {
    Arrays.sort(meetings, Comparator.comparingInt(a -> a[1]));
    int end = Integer.MIN_VALUE, answer = 0;

    for (int[] meeting : meetings) {
        if (meeting[0] < end) {
            continue;
        }

        answer++;
        end = meeting[1];
    }

    return answer;
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| 종료 시각순 | 어떤 해의 첫 회의를 더 일찍 끝나는 회의로 바꿔도 뒤 회의들은 그대로 가능하다 |
| 시작 시각순 / 짧은 길이순 | 뒤에 가능한 회의를 가장 많이 남긴다는 보장이 없다 |
| `start < end`일 때 제외 | `start == end`는 바로 이어서 진행할 수 있다. 끝점도 겹침으로 보는 문제면 조건 변경 |
| 개수 최대화 | 회의마다 보상이 다르면 가장 일찍 끝나는 선택만으로 최대 보상을 보장하지 않는다 |

`meetings[i] = {시작, 끝}`, 시작 < 끝 · 입력 배열 정렬 · 시간 `O(n log n)`.

**분류** [[00 Maps/Greedy Choices|Greedy Choices]]
