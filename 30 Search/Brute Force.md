---
type: template
tags: ["Search"]
aliases: ["Exhaustive Search"]
category: exhaustive-search
categories: [exhaustive-search]
---

# Brute Force

```text
[1, 2, 3, 4]에서 합 5인 쌍 (아래는 값으로 표시)

(1,2)  (1,3)  (1,4) ✓
       (2,3) ✓ (2,4)
              (3,4)

모든 쌍 한 번씩 검사 → 2개
```

| Step | Pseudocode |
|---|---|
| Enumerate | // 첫 인덱스를 고르고 그 뒤의 인덱스를 모두 확인한다. |
| Check | // 두 값을 더해 목표와 같은지 검사한다. |
| Answer | // 맞는 인덱스 쌍의 수를 누적해 반환한다. |

## Java

```java
static long countPairs(int[] numbers, long target) {
    long count = 0;

    for (int i = 0; i < numbers.length; i++) {
        for (int j = i + 1; j < numbers.length; j++) {
            if ((long) numbers[i] + numbers[j] == target) {
                count++;
            }
        }
    }

    return count;
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| `j = i + 1` | 자기 자신과 짝짓기, 같은 쌍의 역순 중복을 함께 막는다. |
| 더하기 전 `long` | int끼리 먼저 더하면 overflow가 발생한 뒤 long에 들어간다. |
| 같은 값 / 다른 인덱스 | 중복 값이 있어도 인덱스가 다르면 별개의 쌍으로 센다. |

`numbers`에서 서로 다른 두 인덱스의 합이 `target`인 쌍 수. **O(n²)** 시간, **O(1)** 추가 공간.

**분류** [[00 Maps/Exhaustive Search|Exhaustive Search]]
