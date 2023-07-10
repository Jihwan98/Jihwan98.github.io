---
title:  "[Python] itertools. Permutation, Combination. 순열과 조합"
author: JIHWAN PARK
categories: [Coding Test, Python]
tag: [Coding Test, Python, itertools]
math: true
date: 2023-04-11 14:10:10 +0900
last_modified_at: 2023-04-11 14:10:10 +0900
image: "https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/a52bd813-f4df-4b79-a27c-e8f0c39fe7da"
---
> python itertools 공식 문서 : [https://docs.python.org/3/library/itertools.html](https://docs.python.org/3/library/itertools.html)
{: .prompt-info}

# ✅ 순열(Permutation)
순열(Permutation)
: <mark>서로 다른 n개에서 r개를 선택하여 일렬로 나열하는 것. $nPr = n! / (n - r)!$</mark>

파이썬에서는 itertools 라이브러리의 Permutation을 통해 쉽게 구할 수 있다. iterator 형식으로 결과가 나온다.

```python
from itertools import permutations

data = [1, 2, 3]
for x in permutations(data, 2):
    print(x)
print('-' * 20)
print(list(permutations(data, 2)))
print('-' * 20)
print(permutations(data, 2).__next__())

'''output
>>> (1, 2)
>>> (1, 3)
>>> (2, 1)
>>> (2, 3)
>>> (3, 1)
>>> (3, 2)
>>> --------------------
>>> [(1, 2), (1, 3), (2, 1), (2, 3), (3, 1), (3, 2)]
>>> --------------------
>>> (1, 2)
'''
```

# ✅ 조합(Combination)
조합(Combination)
: <mark>서로 다른 n개에서 순서에 상관없이 서로 다른 r개를 선택하는 것. $nCr = n! / r! x (n - r) !$</mark>

파이썬에서는 itertools 라이브러리의 Combination을 통해 쉽게 구할 수 있다. iterator 형식으로 결과가 나온다.

```python
from itertools import combinations

data = [1, 2, 3]
for x in combinations(data, 2):
    print(x)
print('-' * 20)
print(list(combinations(data, 2)))
print('-' * 20)
print(combinations(data, 2).__next__())

'''output
>>> (1, 2)
>>> (1, 3)
>>> (2, 3)
>>> --------------------
>>> [(1, 2), (1, 3), (2, 3)]
>>> --------------------
>>> (1, 2)
'''
```