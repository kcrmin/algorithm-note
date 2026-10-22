---
type: template
aliases: ["Subset Enumeration"]
category: exhaustive-search
categories: [exhaustive-search]
---

# Bitmask

```text
arr = [2, 5, 7]

비트 번호      2  1  0    ← 오른쪽 끝이 index 0
해당 원소      7  5  2
mask = 110     1  1  0    → 인덱스 {1,2} → 합 12

000 → 001 → 010 → … → 111
 ∅                    전체
      모든 선택을 한 번씩
```

| Step | Pseudocode |
| --- | --- |
| Enumerate | // 아무것도 고르지 않은 상태부터 모든 선택 조합을 순회한다. |
| Select | // 켜진 비트의 인덱스에 해당하는 값만 합친다. |
| Count | // 합이 목표와 같으면 조합 개수를 하나 늘린다. |

## Java

```java
long countSubsets(int[] arr, long target) {
    int n = arr.length;
    long answer = 0;

    for (int mask = 0; mask < (1 << n); mask++) {
        long sum = 0;
        for (int i = 0; i < n; i++) {
            if ((mask & (1 << i)) != 0) {
                sum += arr[i];
            }
        }
        if (sum == target) {
            answer++;
        }
    }
    return answer;
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| 비트와 인덱스 | 오른쪽 끝이 0번 비트다. `1 << i`는 i번 인덱스의 선택 여부를 가리킨다. |
| `!= 0` | 비트 검사 결과는 1이 아니라 2^i일 수 있다. `== 1`로 검사하지 않는다. |
| 빈 집합 | mask=0도 순회한다. target=0이면 빈 집합도 답에 포함된다. |
| 중복 값 / 크기 | 값이 같아도 인덱스가 다르면 별도 선택. 이 템플릿은 n≤20; 후보가 2ⁿ개로 늘어난다. |

반환: 합이 target인 부분집합 개수 · 시간 O(n·2ⁿ), 추가 공간 O(1).

**분류** [[00 Maps/Exhaustive Search|Exhaustive Search]]
