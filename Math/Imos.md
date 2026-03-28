# Math: Imos


### 소개

> 구간 업데이트를 효율적으로 처리하기 위한 알고리즘
> 일일이 더하는 게 아니라, 어디서 시작해서 어디서 끝나는지만 기록하고 마지막에 한 번에 반영하는 느낌

### 핵심 아이디어


<aside>
💡 
전체를 건드리지 말고 boundary만 건드려라

[l, r] 구간에 +k 하고 싶다?

- diff[l] += k  
- diff[r+1] -= k  

이렇게만 해두고 마지막에 prefix sum 돌리면  
자동으로 구간 반영됨

</aside>

### 특징

- 구간 업데이트: O(1) 
- 마지막 누적합: O(N)
- 여러 번 업데이트할 때 압도적으로 유리
- 대신 중간 query는 잘 못함 
- prefix sum이랑 거의 세트로 나옴


### 필요 자료구조
- 배열 (원본, 변화 기록용)
- prefix sum


### 심화 / 유형
- 2D Imos
    (r1, c1) 에 +k
    (r1, c2+1) 에 -k
    (r2+1, c1) 에 -k
    (r2+1, c2+1) 에 +k
    -> 가로, 세로 누적 해주면 직사각형 내부에만 값이 살아남음

### 시간복잡도

<aside>
💡
- 업데이트 M번: O(M)
- 누적합: O(N)

=> total: O(N + M)

(기존 naive: O(N × M))

</aside>

### 구현 코드 (Kotlin)

```kotlin

```

### 구현 코드 (Python)

```cpp
def apply_range_updates(values, operations):
    n = len(values)
    diff = [0] * (n + 2)

    # 1. mark changes
    for left, right, increment in operations:
        diff[left] += increment
        diff[right + 1] -= increment

    # 2. restore actual values via prefix sum
    current = 0
    for i in range(n):
        current += diff[i]
        values[i] += current

    return values
```

### 구현 코드 (C++)

```cpp

```