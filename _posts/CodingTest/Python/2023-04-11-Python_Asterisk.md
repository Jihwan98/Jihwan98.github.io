---
title:  "[Python] 파이썬의 Asterisk(*)"
author: JIHWAN PARK
categories: [Coding Test, Python]
tag: [Coding Test, Python, Asterisk]
math: true
date: 2023-04-11 14:29:15 +0900
last_modified_at: 2023-04-11 14:29:15 +0900
image: "https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/a52bd813-f4df-4b79-a27c-e8f0c39fe7da"
---
> 1. 곱셈 및 거듭제곱 연산
2. 리스트 확장
3. 가변인자
4. Unpacking
{: .prompt-info}

파이썬에서 * (별표, Asterisk)는 다양한 방법으로 사용되는데, 이에 대해 알아보자.

# ✅ 곱셈 및 거듭제곱 연산
다들 알다시피 기본적으로 곱셈 및 거듭제곱 연산으로 사용한다.

```python
>>> 2 * 3
6
>>> 2 ** 3
8
```

# ✅ 리스트 확장
리스트형 컨테이너 타입에서 데이터를 반복적으로 확장하기 위해 사용한다.

```python
list1 = [0] * 10
tuple1 = (1, ) * 10

list2 = [1, 2, 3] * 3
tuple2 = (1, 2) * 3

print(list1)
print(tuple1)
print(list2)
print(tuple2)

>>> [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
>>> (1, 1, 1, 1, 1, 1, 1, 1, 1, 1)
>>> [1, 2, 3, 1, 2, 3, 1, 2, 3]
>>> (1, 2, 1, 2, 1, 2)
```

# ✅ 가변인자
들어오는 인자의 개수를 모르거나, 어떠한 인자라도 받아서 처리해야하는 경우 사용한다. 파이썬 코드들을 보면 `*args`, `**kwargs`를 사용한 코드를 볼 수 있는데, 이 부분이 바로 가변인자를 받는 부분이다.

파이썬에서 인자의 종류로 **positional arguments**와 **keyword arguments**가 존재한다. **positional arguments**는 위치에 따라 정해지는 인자로 `*`을 사용하고, **keyword arguments**는 키워드를 갖는 인자로 `**`을 사용한다.

## ✔ positional arguments
```python
def print_info(*args):
    print(args)
    print(type(args))

print_info('홍길동', '25', '남자')

'''output
>>> ('홍길동', '25', '남자')
>>> <class 'tuple'>
'''
```

## ✔ keyword arguments
```python
def print_info(**kwargs):
    print(kwargs)
    print(type(kwargs))

print_info(name='홍길동', age='25', gender='남자')

'''output
>>> {'name': '홍길동', 'age': '25', 'gender': '남자'}
>>> <class 'dict'>
'''
```

## ✔ positional arguments & keyword arguments
positional arguments 와 keyword arguments를 같이 사용할 때는, positional arguments가 먼저 나와야한다. 순서가 반대로 될 경우 error가 발생한다. positional arguments는 생략이 불가능하며 정해진 위치에 인자를 전달해야 하지만, keyword arguments는 default 값을 설정할 수 있어 생략할 수 있기 때문이다.

```python
def print_info(*args, **kwargs):
    print(args)
    print(kwargs)
print_info('홍길동', '25', gender='남자')

'''output
>>> ('홍길동', '25')
>>> {'gender': '남자'}
'''
```

# ✅ Unpacking
컨테이너 타입의 데이터를 Unpacking 할 때 사용한다.

```python
numbers = [1, 2, 3, 4, 5]
print(*numbers)

>>> 1 2 3 4 5
```

다른 변수에 가변적으로 Unpacking 할 때 사용한다.

```python
numbers = [1, 2, 3, 4, 5]
*a, = numbers
>>> a = [1, 2, 3, 4, 5]

*a, b = numbers
>>> a = [1, 2, 3, 4]
>>> b = 5

a, *b = numbers
>>> a = 1
>>> b = [2, 3, 4, 5]

a, *b, c = numbers
>>> a = 1
>>> b = [2, 3, 4]
>>> c = 5
```
