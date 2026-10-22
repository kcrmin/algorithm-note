---
type: template
aliases: ["Longest Increasing Subsequence"]
category: dynamic-programming
categories: [dynamic-programming, binary-search]
---

# LIS

```text
입력: 3 → 1 → 2 → 5 → 4

읽은 값     tails
   3        [3]
   1        [1]          길이 1의 끝값을 더 작게
   2        [1, 2]       길이 2로 확장
   5        [1, 2, 5]    길이 3으로 확장
   4        [1, 2, 4]    길이 3은 유지, 끝값만 더 작게

길이별 최소 끝값을 남긴다 → 다음 값을 붙이기 쉬워진다 → 답 3
```

| Step | Pseudocode |
| --- | --- |
| State | // 각 길이에서 만들 수 있는 가장 작은 끝값을 저장한다 |
| Find | // 현재 값 이상인 첫 끝값을 찾는다 |
| Replace | // 그 자리를 현재 값으로 바꿔 다음 값이 붙을 여지를 늘린다 |
| Extend | // 기존 끝값보다 모두 크면 새 길이를 추가한다 |
| Answer | // 만들어진 최대 길이를 반환한다 |

## Strictly Increasing — Java

```java
int lis(int[] arr) {
    int[] tails = new int[arr.length];
    int size = 0;

    for (int value : arr) {
        int left = 0, right = size;

        while (left < right) {
            int mid = left + (right - left) / 2;
            if (tails[mid] >= value) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }

        tails[left] = value;
        if (left == size) {
            size++;
        }
    }

    return size;
}
```

## Non-decreasing

| 목표 | 교체할 첫 위치 | 입력 `[2, 2]`의 길이 |
| --- | --- | --- |
| Strictly Increasing: `<` | 현재 값 **이상**, `>=` | 1: 같은 값은 같은 자리에 덮어씀 |
| Non-decreasing: `≤` | 현재 값 **초과**, `>` | 2: 같은 값 뒤에 붙임 |

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| `tails[i]` | 길이 **i+1**인 부분 수열의 최소 끝값. 배열 인덱스와 길이는 1 차이 |
| `right = size` | 찾는 위치가 맨 뒤이면 새 길이. 아직 안 채운 `tails` 부분은 검색하지 않는다 |
| 길이 / 실제 수열 | 이 코드는 길이만 구한다. 끝값들이 서로 다른 과거 수열에서 왔을 수 있어 `tails` 전체가 원본의 부분 수열인 것은 아니다 |

빈 배열의 답은 0 · 시간 `O(n log n)`, 공간 `O(n)`.

**분류** [[00 Maps/Dynamic Programming|Dynamic Programming]]

**사용 원리** [[10 Java/Array Binary Search|Array Binary Search]]
