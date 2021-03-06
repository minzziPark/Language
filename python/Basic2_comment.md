# [Python] 파이썬 기초 - 주석처리
코드를 짤 때 중요한 요소 중 하나는 주석이라고 생각한다. 
주석을 잘 이용하면 잘 이용할수록 그 코드는 이해하기 쉽고 코딩을 하면서도 유용하게 사용된다. 

- - -
파이썬에서는 주석을 어떻게 사용할까?

1. __한 줄을 주석처리 하고 싶은 경우__ 
` # 주석처리 하고 싶은 코드` 
```
>>> a = 100
>>> # a = 50
>>> print(a)
100
```

#이후에 적힌 그 줄의 모든 코드는 모두 주석 처리가 된다. 

2. __여러 줄을 주석처리 하고 싶은 경우__
`""" 주석처리 하고 싶은 코드 """
or
''' 주석처리 하고 싶은 코드'''`

```
# 주석처리 하지 않은 코드
>>> a = 100
>>> for i in range(5): # 5번 연속으로 a에 10을 계속 더해주겠다는 부분이다.
    a += 10
>>> print(a)
150
```

주석처리 하지 않았을 때의 결과값이다.

```
# 주석처리를 한 코드
>>> a = 100
>>> """
for i in range 5:
    a += 10
"""
>>> print(a)
100
```
