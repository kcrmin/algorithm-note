---
type: template
tags: [심화]
aliases: [LIS]
---

# LIS

LIS는 원래 순서를 지키며 고른 엄격히 증가하는 부분 수열 중 가장 긴 길이다.

> **기억:** 현재 값 이상인 첫 자리 찾기 → 그 자리를 교체 → 끝이면 길이 증가.

**전제:** tails[i]=길이 i+1인 증가 수열의 가능한 최소 끝값. O(n log n).

## 템플릿

```java
int[] tails = new int[arr.length];
int size = 0;
for (int value : arr) {
    int left = 0, right = size;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (tails[mid] >= value) right = mid;
        else left = mid + 1;
    }
    tails[left] = value;
    if (left == size) size++;
}
```

## 왜 이렇게 쓰나

- 같은 길이 수열이라면 끝값이 작을수록 나중에 붙일 값이 많다. 그래서 더 작은 끝값으로 교체한다.
- 이미 있는 값과 같을 때 교체해야 같은 값으로 길이를 늘리지 않는다. 따라서 `>=`인 첫 자리를 찾는다.
- tails는 길이별 최소 끝값을 모은 것이며 그 자체가 실제 선택 수열인 것은 아니다.

`2,5,3,4` → tails는 `[2] → [2,5] → [2,3] → [2,3,4]`. 길이는 3이다.

> [!info]- 더 자세한 설명
>
> `tails[i]`는 길이 i+1인 증가 수열에서 만들 수 있는 **가장 작은 끝값**이다. 같은 길이라면 끝값이 작아야 다음 큰 값을 붙이기 유리하다.
>
> `[2,5,3,4]`를 읽으면 `tails`는 `[2] → [2,5] → [2,3] → [2,3,4]`가 되어 길이 3이다. 3이 들어왔을 때 5를 바꾸는 것은 길이를 줄이는 일이 아니다. 길이 2의 후보를 더 확장하기 쉬운 끝값 3으로 개선한다.
>
> `[2,2]`에서 두 번째 2는 첫 2의 자리를 바꾼다. `>=`로 첫 자리를 찾으므로 길이가 늘지 않아 답은 1이다. 같은 값도 허용하는 비감소 수열은 첫 `> value` 위치를 찾아야 한다.
>
> ### 주의점
>
> - `tails` 전체가 실제로 선택된 한 수열인 것은 아니다. 이 코드는 길이만 구한다.
> - 입력을 정렬하면 원래 인덱스 순서가 사라져 다른 문제가 된다.
> - 빈 배열은 size=0을 반환한다.

> [!example]- 준비 코드 · 전체 구현
>
> 필요한 import와 초기화까지 포함한 전체 코드다. 각 코드 블록은 별도 예시다.
>
> ```java
> class LisTemplate {
>     static int length(int[] arr) {
>         int[] tails = new int[arr.length];
>         int size = 0;
>         for (int value : arr) {
>             int left = 0;
>             int right = size; // [left, right), size는 끝 다음 위치
>             while (left < right) {
>                 int mid = left + (right - left) / 2;
>                 if (tails[mid] >= value) right = mid;
>                 else left = mid + 1;
>             }
>             tails[left] = value;
>             if (left == size) size++;
>         }
>         return size;
>     }
> }
> ```

**관련:** [[배열 이분 탐색]] · [[이분 탐색]] · [[DP]]
