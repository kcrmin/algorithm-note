---
type: template
tags: ["Ranges"]
aliases: ["Two_Pointers", "Two Pointers"]
category: two-pointers-windows
categories: [two-pointers-windows]
---

# Two Pointers - Pair Sum

```text
target = 9
 1  2  4  7  9
 L           R    1+9=10 → R 왼쪽
 L        R       1+7= 8 → L 오른쪽
    L     R       2+7= 9 → found
```

## Steps

| Step | Pseudocode |
| --- | --- |
| **Check** | // 양 끝의 합을 목표와 비교한다. |
| **Increase** | // 합이 작으면 왼쪽을 버린다. 지금 가장 큰 오른쪽과 더해도 작기 때문이다. |
| **Decrease** | // 합이 크면 오른쪽을 버린다. 지금 가장 작은 왼쪽과 더해도 크기 때문이다. |
| **Answer** | // 합이 같으면 찾았다. 두 포인터가 만나면 가능한 쌍이 없다. |

## Java

```java
int left = 0;
int right = arr.length - 1;

while (left < right) {
    long sum = (long) arr[left] + arr[right];

    if (sum == target) {
        return true;
    }

    if (sum < target) {
        left++;
    } else {
        right--;
    }
}

return false;
```

정렬된 배열에서 두 원소 합=target의 존재 여부 · `O(n)` · 정렬 비용 별도

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| 오름차순 | 작게/크게 이동한다는 근거가 정렬에서 나온다. 음수 자체는 문제없다. |
| `left < right` | 서로 다른 두 인덱스를 골라야 한다. 같은 원소를 두 번 사용하지 않는다. |
| 덧셈 전 `(long)` | int끼리 먼저 더하면 overflow 뒤에 long으로 바꿔도 복구할 수 없다. |
| 쌍의 개수 문제 | 이 코드는 존재 여부다. 개수를 세려면 중복 값의 개수까지 별도로 처리해야 한다. |

---

**분류** [[00 Maps/Two Pointers and Windows|Two Pointers and Windows]]
