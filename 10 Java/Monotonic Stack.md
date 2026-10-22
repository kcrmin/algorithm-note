---
type: template
tags: ["DataStructures"]
category: core-collections
categories: [core-collections]
---

# Monotonic Stack

```text
index       0  1  2
value       2  1  3

bottom [0:2, 1:1] top    인덱스:값, 값은 감소 방향
new value = 3
         1:1 → 답 2     1보다 큰 첫 값은 index 2
         0:2 → 답 2     2보다 큰 첫 값도 index 2
bottom [2:3] top        현재 원소가 다음 큰 값을 기다림
```

## Steps

| Step | Pseudocode |
| --- | --- |
| **State** | // 아직 오른쪽의 더 큰 값을 못 찾은 인덱스를 쌓는다. |
| **Resolve** | // 새 값보다 작은 대기 원소를 꺼내고, 새 값의 위치를 답으로 기록한다. |
| **Push** | // 현재 인덱스도 다음 큰 값을 기다리게 한다. |

## Java

```java
int[] answer = new int[arr.length];
Arrays.fill(answer, -1);
Deque<Integer> stack = new ArrayDeque<>();

for (int i = 0; i < arr.length; i++) {
    while (!stack.isEmpty() && arr[stack.peek()] < arr[i]) {
        answer[stack.pop()] = i;
    }

    stack.push(i);
}
```

각 원소 오른쪽의 첫 큰 값 **인덱스**를 answer에 저장 · 값이 필요하면 기록 시 arr[i] 사용

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| 값 대신 인덱스 | 어느 원소의 답을 채울지 알아야 한다. 비교할 값은 arr에서 읽는다. |
| 오른쪽의 **첫** 큰 값 | 대기하는 동안 더 큰 값을 만난 적이 없다. 지금 만난 값이 첫 번째다. |
| `<` / `<=` | 엄격히 큰 값을 찾으면 `<`. 크거나 같은 값을 찾으면 `<=`로 바꾼다. |
| 남은 원소 → `-1` | 끝까지 해결되지 않은 원소에는 오른쪽 큰 값이 없다. |
| `O(n)` | 각 인덱스는 한 번 들어가고 최대 한 번 나온다. |

---

**분류** [[00 Maps/Core Collections|Core Collections]]
