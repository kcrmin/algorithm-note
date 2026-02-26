---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# 섹션 6 - Sorting Searching

## 1. Sorting 기초

### 기본 조작
| 코드                                                    | 요약 / 결과 | 예시      |
|:------------------------------------------------------|:-------:|:--------|
| `Arrays.sort(arr);`                                   | 오름차순 정렬 | `[1,2,3]` |
| `Collections.sort(list);`                             | 오름차순 정렬 | `[1,2,3]` |
| `Collections.sort(list, Collections.reverseOrder());` | 내림차순 정렬 | `[3,2,1]` |
> ❌`Arrays.sort(arr, Collections.reverseOrder());`는 불가능 (객체 타입만 내림차순 정렬 가능)

### 객체 정렬 (Comparator)

```java
import java.util.*;

public class Main {
    class Person {
        String name;
        int age;

        public Person(String name, int age) {
            this.name = name;
            this.age = age;
        }
    }

    public static void main(String[] args) {
        List<Person> list = Arrays.asList(
                new Person("David", 30),
                new Person("Jessica", 20)
        );
        
        // 오름차순 정렬 1
        list.sort((a, b) -> a.age - b.age);
        
        // 문자열 오름차순 정렬 2
        list.sort(Comparator.comparing(p -> p.name));
        
        // 숫자 내림차순 정렬
        list.sort(Comparator.comparingInt(p -> p.age).reversed());
    }
}

```
| 코드                                                           | 요약 / 결과  | 예시                       |
|:-------------------------------------------------------------|:--------:|:-------------------------|
| `list.sort((a, b) -> a.age - b.age);`                        | 숫자 오름차순  | `[Jessica:20, David:30]` |
| `list.sort((a, b) -> b.age - a.age);`                        | 숫자 내림차순  | `[David:30, Jessica:20]` |
| `list.sort(Comparator.comparing(p -> p.name));`              | 문자열 오름차순 | `[David:30, Jessica:20]` |
| `list.sort(Comparator.comparingInt(p -> p.age).reversed());` | 숫자 내림차순  | `[Jessica:20, David:30]` |

### 객체 정렬 (보너스)
```java
public class Main {
    public static class Point {
        int x;
        int y;
        
        public Point(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }
    
    public static void main(String[] args) {
        List<Point> list = Arrays.asList(
                new Point(2, 5),
                new Point(2, 3),
                new Point(1, 4)
        );
        
        // x 오름차순, x 같으면 y 오름차순
        list.sort(Comparator.comparingInt((Point p) -> p.x)
                .thenComparingInt(p -> p.y));
    }
}
```
| 코드                           |           요약 / 결과           | 예시                    |
|:-----------------------------|:---------------------------:|:----------------------|
| `thenComparingInt(p -> p.y)` | 1차 정렬(x)이 같을 때 y 기준으로 2차 정렬 | `(2,3), (2,5)` → y 오름차순 |
> thenComparing 으로 tie-break (2차 비교)

## 2. Searching 기초

### 이진 탐색
```java
public class Main {
    public static int binarySearch(int[] sortedArr, int target) {
        int result = 0;
        int left = 0, right = sortedArr.length - 1;
        
        while (left <= right) {
            int mid = (left + right) / 2;
            
            if (target == sortedArr[mid]) return mid;
            if (target < sortedArr[mid]) right = mid - 1;
            else left = mid + 1;
        }
        
        return -1;
    }
}
```

### 결정 알고리즘
```java
public class Main {
    public static int solutionMax(int lo, int hi) { // hi는 가능한 최대 정답
        int ans = -1;
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            if (verify(mid)) {        // mid가 "가능"
                ans = mid;            // 후보 저장
                lo = mid + 1;         // 더 큰 값 도전
            } else hi = mid - 1;         // 불가능 → 줄이기
        }
        return ans; // 없으면 -1
    }

    public static int solutionMin(int lo, int hi) { // hi는 가능한 최대 정답
        int ans = -1;
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            
            if (verify(mid)) {        // mid가 "가능"
                ans = mid;            // 최소값 갱신
                hi = mid - 1;         // 더 작은 값 도전
            } else lo = mid + 1;         // 불가능 → 키우기
        }
        return ans; // 없으면 -1
    }
}
```

## 3. 주의 포인트
|      코드 / 개념       | 요약                          |
|:------------------:|:----------------------------|
|    binarySearch    | 이진 탐색 전 반드시 sort 필요         |
|   Arrays.sort()    | 내부적으로 primitive일때 사용 가능     |
| Collections.sort() | 내부적으로 Timsort 사용 O(N log N) |


## **핵심 요약**
- 정렬
    - 선택 정렬: 최솟값 찾아서 스왑 O(n^2) / 가장 작은 값을 찾아 순서대로 넣는것
    - 버블 정렬: 인접한 값과 비교 후 스왑 O(n^2) / 가장 큰 값을 뒤로 미루는 것
    - 삽입 정렬: 왼쪽은 정렬된 값만, i의 값을 적절한 위치에 삽입
    - 배열 정렬: `Arrays.sort(arr);`
    - 객체 정렬: `Collections.sort(list)`, `list.sort(Comparator.comparing(p -> p.name));`
- 검색
  - 이분 검색: 투포인터를 사용
  - 결정 알고리즘: 이분 검색 사용

## 문제 다시 풀기
### **[Inflearn](../../src/inflearn/section06_sorting_searching)**
| 분류  | 문제 이름                                                                                        | 포인트 메모                 |
| :-: | :------------------------------------------------------------------------------------------- | :--------------------- |
| 기초  | [06-01 선택 정렬](../../src/inflearn/section06_sorting_searching/INF_S06_P01.java)               | 최솟값으로 정렬               |
| 기초  | [06-02 버블 정렬](../../src/inflearn/section06_sorting_searching/INF_S06_P02.java)               | 인접한 값으로 정렬 (최대값)       |
| 기초  | [06-03 삽입 정렬](../../src/inflearn/section06_sorting_searching/INF_S06_P03.java)               | i를 기준으로 왼쪽은 정렬된 값만     |
| 고난도 | [06-04 Least Recently Used](../../src/inflearn/section06_sorting_searching/INF_S06_P04.java) | ArrayList 없이 int[]로 풀기 |
| 응용  | [06-07 좌표 정렬](../../src/inflearn/section06_sorting_searching/INF_S06_P07.java)               | Comparator 사용          |
| 기초  | [06-08 이분검색](../../src/inflearn/section06_sorting_searching/INF_S06_P08.java)                | 투포인터 사용                |
| 고난도 | [06-09 뮤직비디오(결정알고리즘)](../../src/inflearn/section06_sorting_searching/INF_S06_P09.java)       | 결정 알고리즘 사용             |
| 고난도 | [06-10 마구간 정하기(결정알고리즘)](../../src/inflearn/section06_sorting_searching/INF_S06_P10.java)     | 결정 알고리즘 사용             |