---
type: template
tags: ["Java"]
category: java-essentials
categories: [java-essentials]
---

# Java Basics

```text
size       int[] → length     String → length()     List → size()
equality   "abc"  ── equals ── "abc"                  내용 비교
arithmetic int × int → int 계산 → overflow 가능
           long × int → long 계산                    먼저 변환
```

## Steps

| Step | Pseudocode |
| --- | --- |
| **Size** | // 배열, 문자열, 컬렉션에 맞는 길이 접근법을 고른다. |
| **Compare** | // 문자열의 내용이 같은지 비교한다. |
| **Promote** | // 큰 수 계산은 연산 전에 한쪽을 long으로 바꾼다. |

## Java

```java
int n = arr.length;
int m = s.length();
int size = list.size();

boolean same = s.equals(t);

long sum = (long) a + b;
long product = (long) a * b;
```

`arr`=int[], `s,t`=String, `list`=List, `a,b`=int

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| `==` / `equals` | 객체의 ==는 같은 객체인지, String의 equals는 내용이 같은지 비교한다. |
| null 가능성 | s가 null이면 s.equals는 예외다. 둘 다 null일 수도 있다면 Objects.equals(s,t)를 쓴다. |
| 캐스팅 위치 | `(long) (a*b)`는 int 곱셈이 끝난 뒤 변환한다. `(long) a*b`처럼 계산 전에 바꾼다. |
| 배열 내용 비교 | 배열의 equals는 내용 비교가 아니다. 1차원은 Arrays.equals, 중첩 배열은 Arrays.deepEquals를 쓴다. |

---

**분류** [[00 Maps/Java Essentials|Java Essentials]]
