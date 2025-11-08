- 마지막 업데이트: 2025-09-26
- 상태: 초안

# 개요
이진 검색(Binary Search)은 정렬된 순차 자료구조에서 대상 값을 효율적으로 찾기 위해 탐색 범위를 절반씩 줄여 나가는 알고리즘입니다. 비교 연산 횟수가 `O(log n)`으로 제한되므로 대용량 데이터에서도 빠르게 동작하지만, 전제 조건을 충족하지 않으면 오동작하거나 선형 검색만큼 시간이 걸립니다.

## 전제 조건
- **전순서(총합 순서) 정의**: 데이터가 하나의 비교 기준(오름차순/내림차순)을 공유해야 하며, 다중 키 정렬 시 우선순위가 명확해야 합니다.
- **정렬 상태 유지**: 삽입/삭제 후에도 정렬이 유지되어야 하며, 정렬 알고리즘의 안정성 여부와 무관하게 탐색 시점에는 불변식이 성립해야 합니다.
- **랜덤 접근 가능 구조**: 배열, 슬라이스, 벡터처럼 인덱스 접근(`O(1)`)이 가능한 구조에 적합합니다. 단순 연결 리스트는 포인터 이동이 `O(n)`이므로 비효율적입니다.
- **동형 비교 가능성**: 탐색 키와 자료형 간 비교 연산(`==`, `<`, `>`)이 정의되어 있어야 하며, 지역화/정규화가 필요한 문자열은 사전 처리 후 검색합니다.

> **참고**: Python `bisect` 모듈, C++ `std::lower_bound`/`upper_bound`, Java `Arrays.binarySearch` 등 표준 라이브러리 구현은 위 조건을 가정합니다.

## 절차
1. `left = 0`, `right = n-1` 초기화.
2. 반복(`left <= right`)하면서 `mid = (left + right) // 2` 계산.
3. 비교 결과에 따라 `left = mid + 1` 또는 `right = mid - 1`로 범위를 절반으로 줄임.
4. 일치하면 인덱스를 반환하고, 탐색 실패 시 `-1` 또는 삽입 위치를 반환.

```python
from typing import Sequence, TypeVar

T = TypeVar("T")

def binary_search(items: Sequence[T], target: T) -> int:
    """정렬된 시퀀스에서 대상 값을 찾으면 인덱스를, 없으면 -1을 반환."""
    left, right = 0, len(items) - 1
    while left <= right:
        mid = left + (right - left) // 2  # overflow 방지 패턴
        probe = items[mid]
        if probe == target:
            return mid
        if probe < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

### 중복 처리 전략
- **첫 번째(왼쪽) 위치**: 값이 일치할 때도 `right = mid - 1`로 이동하여 마지막으로 찾은 인덱스를 저장 (`lower_bound`).
- **마지막(오른쪽) 위치**: 값이 일치하면 `left = mid + 1`로 이동 (`upper_bound - 1`).
- **존재 여부 + 삽입 위치**: 탐색 종료 후 `left`가 삽입해야 할 인덱스가 되므로 정렬된 컬렉션 유지에 활용.

## 복잡도
- 시간 복잡도: `O(log n)` — 절반 분할을 반복.
- 공간 복잡도: 반복 구현 시 `O(1)`, 재귀 구현은 호출 스택 `O(log n)`.

## 구현 시 주의 사항
- **정수 오버플로**: `mid = left + (right - left) // 2` 패턴을 사용해 32비트 환경에서 오버플로를 방지.
- **범위 수축 불변식**: 비교 후에 반드시 탐색 범위가 줄어드는지 (`left` 증가 또는 `right` 감소) 확인.
- **정렬 검증**: ETL/데이터 파이프라인에서는 탐색 전에 샘플링 검사를 수행하거나 정렬 실패 시 예외를 발생시키는 방어 로직을 추가.
- **캐시 효율**: 큰 데이터셋에서는 분기 예측 실패가 성능에 영향을 줄 수 있으므로, SIMD-friendly 검색(Branchless Binary Search) 또는 Galloping Search를 고려.

## 참고 자료
- Cormen et al., *Introduction to Algorithms*, 3rd ed., Chapter 2.3.
- MIT OCW 6.006, *Lecture 2: Growth of Functions* (이진 검색 정확성 증명 포함).
- Oracle Java Documentation, `Arrays.binarySearch` 메서드 설명.
