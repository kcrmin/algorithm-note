---
type: template
cssclasses: [algorithm-reference]
tags: [심화]
aliases: [LIS]
---

# LIS

> **핵심:** `tails`에서 현재 값 이상인 첫 자리를 바꾼다. 끝에 붙으면 길이가 늘어난다.

## 개념

LIS는 원래 인덱스 순서를 지키며 골랐을 때 값이 엄격히 증가하는 가장 긴 부분 수열이다. 아래 tails[i]는 길이 i+1인 증가 수열의 가능한 끝값 중 최솟값으로, 다음 값을 붙이기 유리한 후보를 남긴다.

## 적용 조건

원래 인덱스 순서를 지키며 **엄격히 증가하는 부분 수열의 길이**를 구한다. 연속할 필요는 없다. 시간 `O(n log n)`, 공간 `O(n)`. 위치 갱신은 [[배열 이분 탐색]]의 lower bound와 같다.

## Java 템플릿

```java
class LisTemplate {
    static int length(int[] arr) {
        int[] tails = new int[arr.length];
        int size = 0;
        for (int value : arr) {
            int left = 0;
            int right = size; // [left, right), size는 끝 다음 위치
            while (left < right) {
                int mid = left + (right - left) / 2;
                if (tails[mid] >= value) right = mid;
                else left = mid + 1;
            }
            tails[left] = value;
            if (left == size) size++;
        }
        return size;
    }
}
```

> [!info]- 원리와 코드 설명
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

**관련 개념:** [[이분 탐색]] · [[DP]]
