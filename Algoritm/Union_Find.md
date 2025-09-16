## 서로소 집합 Union Find

- 서로 공통 원소가 없는 집합 → 서로간 교집합 없음
- 대표자 생성 (부모) → 대표자가 다르면 서로 다른 집합이다.

## 트리 구현

- 하나의 집합이 하나의 트리 → 루트 노드가 집합의 대표자가 된다.

## Make-Set(x)

- 자기 자신을 부모로 하는 집합 형성

```python
# 집합의 수 = n
n = 5
# 자기 자신을 부모로 하는 각 집합을 만든다.
def make_set(n):
    # 1 ~ n 까지의 원소가 자기 자신(대표자)을 가지게 설정
    parents = [i for i in range(n+1)] # 0을 사용하지 않을 것 이므로 n+1개 인덱스 형성
    return parents

# Python -> 리스트 컴프리헨션으로 한 줄로 구현 가능
parents = [i for i in range(n + 1)]
print(parents)
```

## Find-Set(x)

- x가 속한 집합의 대표(부모)를 반환

```python
# 집합의 대표자는?
def find_set(x):
    if x == parents[x]:
        return x

    # 자기자신이 대표가 아니면 부모를 찾아 올라감
    return find_set(parents[x])
```

### Find-Set(x)의 경로 압축

- 부모를 찾는 과정에서 계속해서 트리 부모가 호출 되면 낭비가 발생된다.
- 한번에 대표자를 가리키도록 정리하기

```python
def find_set(x):
    if x == parents[x]:
        return x

    # 경로 압축 (path compression)
    # 내가 대표자를 바로 가리키도록
    parents[x] = find_set(parents[x])
    return parents[x]
```

## Union(x, y)

- x가 속한 집합과 y가 속한 집합을 하나로 묶는다.

```python
# x, y 집합을 합친다.
def union(x, y):
    # x, y의 대표자 설정
    rep_x = find_set(x)
    rep_y = find_set(y)

    # 만약 이미 서로 같은 집합이면? 종료
    if rep_x == rep_y:
        return

    # 정한 조건에 따라 대표자를 연결
    if rep_x < rep_y:
        parents[rep_y] = rep_x
    else:
        parents[rep_x] = rep_y
```

## Rank-Union

- 작은 트리를 큰 트리로 병합
- 트리의 높이 = 랭크
```python
# 1. 각 집합을 만들어주는 함수
def make_set(n):
    # 1 ~ n 까지의 원소가 "각자 자기 자신이 대표자라고 설정"
    parents = [i for i in range(n + 1)]
    ranks= [0] * (n + 1)  # rank 를 모두 0으로 초기화
    return parents, ranks

# 3. 두 집합을 합치는 함수
def union(x, y):
    # 1. x, y 의 대표자를 검색
    rep_x = find_set(x)
    rep_y = find_set(y)

    # 만약 이미 같은 집합
    if rep_x == rep_y:
        return  # 같은 집합끼리는 합칠 필요가 없다

    # 덩치가 더 작은 집합(==rank 가 더 낮은 집합)이 더 큰 집합 밑으로 가야한다.
    if ranks[rep_x] < ranks[rep_y]:
        parents[rep_x] = rep_y
    elif ranks[rep_x] > ranks[rep_y]:
        parents[rep_y] = rep_x
    else:
        # rank 가 동일하다
        # -> 한 쪽으로 병합하고, 대표자의 rank 를 + 1
        parents[rep_y] = rep_x
        ranks[rep_x] += 1
n = 5
parents, ranks = make_set(n)
```