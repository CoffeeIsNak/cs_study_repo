# 해시 테이블(Hash Table)

- 해시 테이블(Hash Table)은 키(Key)와 값(Value)의 쌍을 저장하는 데이터 구조입니다. 

- 데이터를 빠르게 검색, 삽입, 삭제할 수 있도록 설계된 자료구조로, 해시 함수(Hash Function) 를 사용하여 키를 특정한 주소(배열의 인덱스)로 변환하여 저장합니다.




### 1. 해시 테이블의 기본 원리

- 해시 함수(Hash Function) 적용
    - 키(Key)를 입력하면 특정한 해시 값(Hash Value)을 반환하는 함수를 사용합니다.
    - 이 해시 값은 배열(버킷)의 인덱스로 변환되어 해당 위치에 값을 저장합니다.

- 해시 충돌(Hash Collision) 처리
    - 서로 다른 키가 같은 해시 값을 가질 수도 있습니다. 이를 해시 충돌이라고 하며, 충돌을 해결하는 다양한 방법이 존재합니다.




### 2. 해시 테이블의 핵심 개념

1) 해시 함수(Hash Function)

- 키를 해시 테이블의 크기 내의 특정한 인덱스로 변환하는 함수입니다.

- 좋은 해시 함수는 충돌이 적고, 계산 속도가 빠르며, 고르게 분포되는 특징을 가집니다.

- 해시함수 예제

```python
# hash() 는 파이썬 내장 해시 함수
def hash_function(key, table_size):
    return hash(key) % table_size
```


2) 해시 충돌(Hash Collision) 해결 방법

- 해시 충돌이 발생하면, 여러 개의 키가 동일한 인덱스에 저장되면서 문제가 발생할 수 있습니다. 이를 해결하는 방법에는 다음과 같은 기법이 있습니다.

1. 체이닝(Chaining)

- 동일한 해시 값을 가진 데이터를 연결 리스트(Linked List) 로 저장하는 방식입니다.
    - 장점: 충돌이 많아도 성능이 안정적입니다.
    - 단점: 메모리를 추가로 사용해야 합니다.

- Chaining 예시

```python
class HashTable:
    def __init__(self, size):
        self.size = size
        self.table = [[] for _ in range(size)]  # 각 버킷을 리스트로 저장

    def put(self, key, value):
        index = hash(key) % self.size
        self.table[index].append((key, value))  # 충돌 발생 시 리스트에 추가

    def get(self, key):
        index = hash(key) % self.size
        for k, v in self.table[index]:  # 해당 인덱스 리스트를 탐색
            if k == key:
                return v
        return None  # 키가 없으면 None 반환
```

2. 개방 주소법(Open Addressing)

- 충돌이 발생하면 빈 공간을 찾아 데이터를 저장하는 방식입니다.

- 대표적인 기법예시
    - 선형 탐색(Linear Probing): 충돌 발생 시 다음 인덱스(index + 1)를 확인
    - 이차 탐색(Quadratic Probing): 충돌 발생 시 index + i^2 위치를 확인
    - 이중 해싱(Double Hashing): 다른 해시 함수를 한 번 더 적용하여 새로운 인덱스를 찾음

- 선형 탐색(Linear Probing) 예시

```python
class HashTableLinearProbing:
    def __init__(self, size):
        self.size = size
        self.table = [None] * size

    def put(self, key, value):
        index = hash(key) % self.size
        while self.table[index] is not None:  # 충돌 발생 시 다음 인덱스로 이동
            index = (index + 1) % self.size
        self.table[index] = (key, value)

    def get(self, key):
        index = hash(key) % self.size
        while self.table[index] is not None:
            if self.table[index][0] == key:
                return self.table[index][1]
            index = (index + 1) % self.size  # 다음 인덱스 확인
        return None
```




### 3. 해시 테이블의 성능

- 평균적인 경우 (충돌이 적을 때)
    - 검색, 삽입, 삭제: O(1) (상수 시간)

- 최악의 경우 (충돌이 많을 때)
    - 체이닝: O(n) (리스트를 모두 탐색)
    - 개방 주소법: O(n) (빈 공간을 찾기 위해 전체를 탐색)

- 따라서 해시 함수의 품질과 적절한 테이블 크기 선택이 중요합니다.




### 4. 해시 테이블의 활용 예시

1) 데이터 저장 및 조회

- 딕셔너리(Dictionary) 구현: Python의 dict, Java의 HashMap, C++의 unordered_map 등이 해시 테이블을 기반으로 동작합니다.


2) 캐싱(Caching)

- LRU Cache(Least Recently Used Cache): 최근에 사용되지 않은 데이터를 제거하는 방식의 캐시
    - Python의 functools.lru_cache를 사용하여 구현 가능

```python
from functools import lru_cache

@lru_cache(maxsize=100)  # 최근 100개의 결과를 저장
def expensive_function(x):
    print("Computing...")
    return x * x

print(expensive_function(10))  # Computing... 100
print(expensive_function(10))  # 100 (캐시에서 반환)
```


3) 데이터베이스 인덱싱

- 해시 인덱스를 활용하여 데이터베이스에서 빠르게 조회 가능 (예: MySQL의 MEMORY 엔진)


4) 중복 검사 (Duplicate Detection)

- 해시 테이블을 사용하여 리스트에서 중복을 쉽게 제거 가능

```python
numbers = [1, 2, 2, 3, 4, 4, 5]
unique_numbers = list(set(numbers))  # {1, 2, 3, 4, 5}
```


### 5. 해시 테이블 vs 다른 데이터 구조

- 데이터 구조 / 평균 검색 시간 / 최악의 검색 시간 / 삽입 시간 / 삭제 시간

- 배열(Array): O(n) / O(n) / O(n) / O(n)

- 정렬된 배열(Sorted Array): O(log n) / O(log n) / O(n) / O(n)

- 연결 리스트(Linked List): O(n) / O(n) / O(1) / O(1)

- 이진 탐색 트리(BST): O(log n) / O(n) / O(log n) / O(log n)

- 해시 테이블(Hash Table): O(1) / O(n) / O(1) / O(1)

- 해시 테이블은 탐색 속도가 빠르지만, 정렬이 필요할 경우 부적합합니다. => 정렬이 필요한 경우에는 BST (이진 탐색 트리) 가 더 적합할 수 있습니다.




### 6. 결론

- 해시 테이블은 빠른 데이터 검색, 삽입, 삭제가 필요한 경우 매우 유용한 자료구조입니다.

- 하지만 충돌 문제를 해결해야 하며, 정렬이 필요한 경우에는 다른 자료구조를 선택하는 것이 좋습니다.

- Python에서는 dict와 set이 기본적으로 해시 테이블을 기반으로 동작합니다.




### 7. 활용

- 알고리즘 문제를 풀기위해 필수적으로 알아야 할 개념, => 브루트 포스(완전 탐색)으로는 시간초과에 빠지게 되는 문제에서는 해시를 적용시켜야 한다고 함.

- 예제 문제 => 이에 대한 해결을 중심으로 지금부터는 설명

---

N(1~100000)의 값만큼 문자열이 입력된다.

처음 입력되는 문자열은 "OK", 들어온 적이 있던 문자열은 "문자열 + index"로 출력하면 된다.

- Input

```
5
abcd
abc
abcd
abcd
ab
```

- Output

```
OK
OK
abcd1
abcd2
OK
```

- 현재 N값은 최대 10만이다. 브루트 포스로 접근하면 N^2이 되므로 100억번의 연산이 필요해서 시간초과에 빠질 것이다. 따라서 **'해시 테이블'**을 이용해 해결해야 한다.

- 입력된 문자열 값을 해시 키로 변환시켜 저장하면서 최대한 시간을 줄여나가도록 구현해야 한다.




### 8. 해시 테이블 구현

- 참고하는 레포에서 위 문제를 위한 해시테이블을 직접 구현했어서 나도 해봄

- 파이썬으로 작성

```python
def get_hash_key(s: str, HASH_VAL: int, HASH_SIZE: int) -> int:
    """문자열을 입력받아서 hashing된 키를 반환하는 함수, O(len(s))"""
    key = 0
    for char in s:
        key = (key * HASH_VAL) + ord(char)
    if key < 0:
        key = -key  # 음수라면 양수로 변환
    return key % HASH_SIZE

class HashTable:
    def __init__(self, hash_size: int, hash_val: int):
        self.hash_size = hash_size
        self.hash_val = hash_val  # 해싱에 참고할 value
        self.table = {}  # 딕셔너리를 그냥 해시테이블로 씀

    def insert(self, string: str) -> str:
        key = get_hash_key(string, self.hash_val, hash_szie)

        # 해시 테이블 안에 해당 키가 없다면
        if key not in self.table:
            self.table[key] = {}  # 새롭게 딕셔너리 생성
        
        # 해당 문자열이 처음이라면
        if string not in self.table[key]:
            self.table[key][string] = 0
            return "OK"
        else:
            # 이미 등장했던 문자열이라면
            self.table[key][string] += 1
            return f"{string}{self.table[key][string]}"

# 입력을 처리할 main
def main():
    import sys
    input = sys.stdin.read
    data = input().split()

    N = int(data[0])  # 문자열의 개수
    strings = data[1:]

    HASH_SIZE = 100003  # 소수를 선택하면 충돌 확률 감소
    HASH_VAL = 31  # 일반적으로 사용되는 해시 값

    hash_table = HashTable(HASH_SIZE, HASH_VAL)

    results = []
    for s in strings:
        results.append(hash_table.insert(s))
    
    print("\n".join(results))


if __name__ == "__main__":
    main()
```
