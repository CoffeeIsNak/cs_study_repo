# 이진 탐색(Binary Search)

- 탐색 범위를 두 부분으로 분할하면서 찾는 방식임.

- 처음부터 끝까지 탐색하는 거 보다 빠름. 대신 정렬이 필요. => O(NlogN) 후 O(logN) => O(NlogN)
    - 전체 탐색 : O(N)
    - 이분 탐색 : O(logN)


### 1. 코드 구현

```python
def binary_search(l, target):
    """어떤 정렬된 리스트를 받아 target을 찾는 함수"""
    # sorted_l = sorted(l) => 외부에서 정렬되있지 않으면 인덱스를 받아도 쓸모없다고 생각해서 제외
    length = len(l)

    start, end = 0, length - 1
    mid = 0  # 그냥 초기화

    while start <= end:
        mid = (start + end) // 2  # 홀수개일때는 정 중앙이 되지만 짝수개일 때는 중앙 쌍에서 왼쪽 값을 가져옴

        if target == sorted[mid]:
            # 딱 맞는 경우
            return mid
        elif sorted[mid] < target:
            # 정렬되어있기 때문에 mid 이하의 인덱스는 탐색할 필요가 없음, 즉 탐색 범위를 반으로 줄일 수 잇음.
            start = mid + 1
        elif sorted[mid] > target:
            # 정렬되어 있기 때문에 mid 이상의 인덱스는 탐색할 필요가 없음, 즉 탐색 범위를 반으로.
            end = mid - 1
    
    return -1  # 위에서 안걸리면 없다는 거임.
```
