---
type: template
cssclasses: [algorithm-reference]
tags: [Java]
---

# Java 기본 도구

> **핵심:** 배열은 `length`, 문자열은 `length()`, 컬렉션은 `size()`. 큰 합은 계산하기 전부터 `long`.

## 개념

Java 알고리즘 구현에서 반복해서 쓰는 배열·문자열·컬렉션·자료형의 기본 규칙이다. 값이 저장되는 자료형뿐 아니라 **계산이 수행되는 자료형**과 인덱스의 범위를 함께 확인한다.

## 적용 조건

배열·문자열을 순회하거나 값을 누적하는 구현에 사용한다.

## Java 템플릿

배열 합을 long으로 누적하는 메서드 예시다.

```java
class Solution {
    public long solution(int[] numbers) {
        long sum = 0;
        for (int i = 0; i < numbers.length; i++) {
            sum += numbers[i];
        }
        return sum;
    }
}
```

문자열을 가공할 때는 다음 도구를 고른다. 이 블록은 메서드 안에서 실행하는 예시다.

```java
String s = "abca";
int count = 0;
StringBuilder result = new StringBuilder();
for (int i = 0; i < s.length(); i++) {
    char ch = s.charAt(i);
    if (ch == 'a') count++;
    result.append(ch);
}
boolean same = result.toString().equals(s);
```

> [!info]- 원리와 코드 설명
>
> `numbers = [10, 20, 30]`이면 인덱스는 `0, 1, 2`다. 길이 `3`은 마지막 인덱스가 아니므로 `i < numbers.length`를 쓴다. `<=`를 쓰면 없는 `numbers[3]`에 접근한다.
>
> `sum += numbers[i]`는 `sum = sum + numbers[i]`다. 값은 `0 → 10 → 30 → 60`으로 쌓인다. `i + 2`는 계산만 하고 버린다. 두 칸씩 움직이려면 `i += 2`여야 한다.
>
> 문자 `'a'`는 `char`, 문자열 `"a"`는 `String`이다. 문자 값은 `==`, 문자열 내용은 `.equals()`로 비교한다. `String`을 반복해서 `+`로 이어 붙이면 중간 문자열이 계속 생길 수 있으므로 반복 조립에는 `StringBuilder`를 쓴다.
>
> ### long은 대입보다 계산 순서가 먼저다
>
> `int a = 2_000_000_000`일 때 `long total = a + a`는 먼저 int 덧셈을 하므로 이미 넘친 값을 저장한다. `(long) a + a`로 첫 피연산자를 바꿔야 long으로 계산한다. 곱셈도 `(long) n * max`다.
>
> ### 격자 좌표는 한 방식만 쓴다
>
> `x`는 열, `y`는 행, 배열은 `matrix[y][x]`다. 높이 2·너비 3이면 유효한 y는 `0~1`, x는 `0~2`다. 방향 탐색은 [[BFS]]의 `dx`, `dy`, `nx`, `ny`로 통일한다.
>
> ### 시간복잡도는 입력 크기부터 판단한다
>
> | 입력 크기 예 | 먼저 검토할 계산량 | 이유 |
> |---|---|---|
> | n = 20 | 부분집합 `2^n` | 약 백만 상태. 상태당 처리량도 확인 |
> | n = 1,000 | `n²` | 약 백만 쌍 |
> | n = 100,000 | `n log n`, `n` | `n²`은 약 100억 쌍 |
>
> 이 표는 접근을 고르는 출발점이다. 통과를 보장하는 초당 연산 공식은 아니다. 문자열 비교·객체 생성·메모리도 계산량에 영향을 준다.
>
> ### 주의점
>
> - `int[]`는 배열이고 `Integer`는 컬렉션의 원소 타입이다. `List<int>`는 쓸 수 없다.
> - 뒤에서 순회할 때는 `i >= 0`. `i >- 1`은 `i > -1`로 읽혀 0도 포함한다.
> - 빈 줄 출력은 `System.out.println()`. 빈 문자 `''`는 문법 오류다.
> - `split(".")`의 점은 정규식이다. 점 자체로 자르려면 `split("\\.")`를 쓴다.
> - 여러 번 호출될 수 있는 `solution`에서는 필드·방문 배열·정답 값을 매번 초기화한다.

**관련 개념:** [[정렬]] · [[해시]]
