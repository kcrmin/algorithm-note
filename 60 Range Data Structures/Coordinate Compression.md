---
type: template
tags: ["DataStructures"]
aliases: ["Coordinate_Compression"]
category: range-queries
categories: [range-queries]
---

# Coordinate Compression

```text
원본        [1000, -50, 1000, 7]
                ↓ 정렬 + 중복 제거
값의 순서   [-50,    7, 1000]
순위        [  0,    1,    2]
                ↓ 원본 순서로 치환
결과        [   2,   0,    2, 1]

-50 < 7 < 1000  →  0 < 1 < 2
같은 값은 같은 번호 / 숫자 사이 거리는 사라짐
```

| Step | Pseudocode |
| --- | --- |
| Copy | // 원본 순서를 보존할 복사본을 만든다 |
| Sort | // 작은 값부터 순서를 정한다 |
| Rank | // 새로운 값에만 다음 번호를 부여한다 |
| Restore | // 원본의 각 값을 그 번호로 바꾼다 |

## Java

```java
int[] compress(int[] arr) {
    int[] sorted = arr.clone();
    Arrays.sort(sorted);

    Map<Integer, Integer> rank = new HashMap<>();
    for (int value : sorted) {
        if (!rank.containsKey(value)) {
            rank.put(value, rank.size());
        }
    }

    int[] result = new int[arr.length];
    for (int i = 0; i < arr.length; i++) {
        result[i] = rank.get(arr[i]);
    }

    return result;
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| 정렬 후 번호 부여 | 처음 등장한 순서로 번호를 주면 원래 대소 관계를 잃는다 |
| 중복 확인 | 같은 값은 같은 순위여야 한다. 중복마다 번호를 늘리지 않는다 |
| 순위 / 거리 | `rank(b)-rank(a)`는 실제 길이 `b-a`가 아니다. 길이·넓이 계산에는 원래 좌표도 보관 |
| Fenwick Tree와 연결 | 결과는 0부터 시작. 1-index인 Fenwick에는 순위에 1을 더해 사용 |

빈 배열도 가능 · 시간 `O(n log n)`, 공간 `O(n)`.

**분류** [[00 Maps/Range Queries|Range Queries]]
