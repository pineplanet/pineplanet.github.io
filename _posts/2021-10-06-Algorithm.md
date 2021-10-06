---
title: TIL 2021-10-06 Programmers 01 주식 가격
tags: [Python,Algorithm]
categories: Algorithm
---
주식 가격이 떨어지지 않은 기간을 저장한 배열을 리턴 하는 함수를 만드는 문제를 풀어보고 있습니다. 

오늘 첫 번째 시도를 하였는데, 정확성 테스트(?)는 성공 하였지만, 효율성 문제로 실패하였습니다. 내일 꼭 성공하고 말겠어요(복수할테다).

- 첫 시도 (실패 코드 )
```python 
def solution(prices):
    answer = []
    while prices:
        for i in range(len(prices)):
            if (prices[0] > prices[i]):
                answer.append(i)
                break
            else:
                if i == len(prices)-1:
                    answer.append(i)


        prices.pop(0);
    return answer
```
- 2차 시도 전략 -> 효율성 문제로 실패! 
    - prices의 길이를 미리 저장해두자.
    - prices.pop(0)을 미리 해서 변수에 저장해두고, 
    - for 문을 range로 하지말고 prices의 데이터들을 직접 써보자. 
        - 숫자 변수를 선언 하고 
        - 주식 가격들을 비교 할 때 숫자 변수를 +1씩 하게 한다음 
        - 마지막에 answer에 append를 해봐야겠다. 
```python
def solution(prices):
    answer = []
    Max = len(prices)
    while prices:
        item = prices.pop(0);

        a = 0;
        for i in prices:
            if item > i:
                a = a+1
                break
            else:
                a =a+1
        answer.append(a)

    return answer
```

- 3차 시도 : 이번엔 리스트 말고 다른걸 써보자. -> 통과 !!!!!!!

``` python 
from collections import deque
def solution(prices):
    prices = deque(prices)
    answer = []
    Max = len(prices)
    while prices:
        item = prices.popleft();

        a = 0;
        for i in prices:
            if item > i:
                a = a+1
                break
            else:
                a =a+1
        answer.append(a)

    return answer
```