---
type: template
aliases: ["Counting Sort", "Bucket"]
category: core-collections
categories: [core-collections]
---

# Frequency Array and Buckets

```text
입력       [3, 1, 3, 2]
                  ↓ 값별로 모으기
값          0  1  2  3
개수        0  1  1  2
                  ↓ 작은 값부터 개수만큼 꺼내기
정렬       [1, 2, 3, 3]
```

| Step | Pseudocode |
| --- | --- |
| Count | // 값을 배열 인덱스로 사용해 등장 횟수를 센다. |
| Scan | // 작은 값부터 모든 값 칸을 순회한다. |
| Restore | // 각 값을 등장 횟수만큼 결과에 넣는다. |

## Java

```java
int[] sort(int[] arr, int maxValue) {
    int[] count = new int[maxValue + 1];
    for (int value : arr) {
        count[value]++;
    }

    int[] result = new int[arr.length];
    int index = 0;
    for (int value = 0; value <= maxValue; value++) {
        for (int repeat = 0; repeat < count[value]; repeat++) {
            result[index++] = value;
        }
    }
    return result;
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| 값 범위 | 0≤arr[i]≤maxValue. 음수를 그대로 인덱스로 쓸 수 없다. |
| `maxValue + 1` | 0부터 maxValue까지 양 끝을 포함하므로 칸이 하나 더 필요하다. |
| 언제 유리한가 | 값 범위 K가 작을 때 사용한다. 원소가 적어도 maxValue가 크면 배열과 순회 비용이 커진다. |
| 빈도 배열 / 정렬 | 개수만 필요하면 count에서 끝낸다. 위 템플릿은 정수값을 복원하며 객체의 안정 정렬용은 아니다. |

maxValue≥0 · K=maxValue+1 · 시간·공간 O(n+K).

**분류** [[00 Maps/Core Collections|Core Collections]]
