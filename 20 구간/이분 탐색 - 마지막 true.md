---
type: template
tags: [구간]
---

# 이분 탐색 - 마지막 true

가능 T에서 불가능 F로 한 번 바뀌는 후보 중 **마지막 true**를 찾는다.

> **기억:** 0부터 max → 위쪽 가운데 → 참은 남겨 오른쪽 → 거짓은 버려 왼쪽.

**전제:** TTTFFF, `valid(0) == true`, `0 <= max < Long.MAX_VALUE`. 검사 비용이 C면 시간 `O(C log(max + 1))`, 공간 `O(1)`.

`max`는 후보 상한이고, `valid(long x)`는 같은 클래스의 판정 메서드다. 답을 반환하는 메서드 안에 쓴다.

## 템플릿

```java
long left = 0, right = max;
while (left < right) {
    long mid = left + (right - left + 1) / 2;
    if (valid(mid)) {
        left = mid;
    } else {
        right = mid - 1;
    }
}
return left;
```

## 왜 이렇게 쓰나

- `left = mid`: 참인 mid를 남기고 더 큰 성공을 찾는다.
- `right = mid - 1`: 거짓인 mid부터 오른쪽까지 전부 버린다.
- mid의 `+1`: 후보가 `[3,4]`이면 4를 검사해야 `left = mid`여도 범위가 줄어든다.

1. `max = 5`, 조건 `x <= 3`: `[0,5]`의 위쪽 `mid = 3`은 T → `[3,5]`.
2. `mid = 4`는 F → `[3,3]`.
3. 후보 하나, 답 `3`. `[3,4]`에서 아래쪽 mid 3을 쓰면 이동하지 않는다.

**관련 개념:** [[이분 탐색]] · [[배열 이분 탐색]]

> [!info]- 더 자세한 설명
> ### 왜 위쪽 mid인가
>
> 첫 true는 `right = mid`로 오른쪽 끝을 남기므로 아래쪽 mid를 쓴다. 마지막 true는 `left = mid`로 왼쪽 끝을 남기므로 위쪽 mid를 쓴다. 남기는 쪽과 mid 방향을 한 쌍으로 기억한다.
>
> 왼쪽 끝이 false면 TTTFFF에서 true가 없다. 아래 전체 메서드는 이때 `-1`을 반환한다. 이 반환값을 쓰므로 전체 메서드도 비음수 후보를 전제로 한다.
>
> `right - left + 1`까지 계산하므로 `0 <= left <= right < Long.MAX_VALUE`를 전제로 한다.
>
> ### upper bound와 이름 구별
>
> `arr = [1,3,3,5]`, target 3에서 마지막 `<= 3` 위치는 2다. 배열의 **upper bound**는 첫 `> 3` 위치인 3이다. [[배열 이분 탐색]]의 upper bound와 이 노트를 같은 이름으로 부르지 않는다.

> [!example]- 준비 코드 · 전체 구현
> 클래스 안에 두는 전체 메서드다.
>
> ```java
> static long lastTrue(long left, long right,
>                      java.util.function.LongPredicate valid) {
>     if (!valid.test(left)) {
>         return -1;
>     }
>
>     while (left < right) {
>         long mid = left + (right - left + 1) / 2;
>
>         if (valid.test(mid)) {
>             left = mid;
>         } else {
>             right = mid - 1;
>         }
>     }
>
>     return left;
> }
> ```
