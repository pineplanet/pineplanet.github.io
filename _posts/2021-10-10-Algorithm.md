---
title: 2021-10-10 Programmers 02 기능개발
tags: [Python,Algorithm]
categories: Algorithm
---
# 문제 
오늘은 프로그래머스 [42586#](https://programmers.co.kr/learn/courses/30/lessons/42586#) 를 풀어 보았습니다. 

입출력 예를 이용하여 문제를 설명해보겠습니다. 

progresses : [93, 30, 55]

speeds : [1, 30, 5]

return : [2, 1]


첫 번째 배열인 progresses 는 하나의 작업을 말하는데, 작업이 진행된 정도를 말합니다. 그러니까 0번지는 93프로 진행이 되있고, 1번지는 30프로, 2번지는 55프로 진행 되었습니다. 

두 번째 배열 speed는 위의 작업 각각을 하루동안 진행할 수 있는 작업 속도를 말합니다. 

`1 번 작업은` 93프로 까지 진행이 되어서 완료까지 7프로가 남았는데, 1일에 1프로의 작업 속도를 낼 수 있으니 작업완료 까지 `7일`이 걸립니다.

`2 번 작업은` 30프로 까지 진행이 되어서 완료까지 70프로가 남았는데, 1일에 30프로의 속도를 낼 수 있으니 작업완료 까지 `3일`이 걸립니다. 

`3 번 작업은` 55프로 까지 진행이 되어서 완료까지 45프로가 남았는데,
1일에 5프로의 작업 속도를 낼 수 있으니 작업완료까지 `9일`이 걸립니다. 

그러니까 2번 작업이 제일 빨리 끝나긴 하는데, 문제는 제한 사항이 있어요. 
뒤의 작업이 먼저 끝나더라도 앞의 작업이 완료 되어서 배포할 수 있는 날에 배포 할 수 있네요. 

그렇다면 1번 작업이 끝나는 날인 7일에 1번과 2번이 함께 배포가 되고, 그 다음 3번 작업이 끝나는 날인 9일에 3번이 배포가 됩니다. 

# 코드

저번 문제에서 배웠던 deque와 popleft()를 이용해서 문제를 풀어보았습니다. 

먼저 인풋으로 받는 progress와 speed를 계산 해서 각 작업이 완료되기까지 필요한 날들을 deque에 저장 했습니다. 그리고 리턴 배열을 생성하기 위한 코드를 작성 했는데요. 

문제를 보면 
1. 앞 기능의 작업일 수가 뒤의 기능의 작업일 수보다 크면 ( 뒷 기능이 더 일찍 끝나면) 뒤의 기능은 앞 기능의 작업일 수 가 끝날 때 같이 배포가 되고, (앞 > 뒤 )

2. 뒤의 기능의 작업일 수가 앞의 기능의 작업일 수 보다 크면 따로 배포 됩니다. (앞 < 뒤)

그래서 저는 처음 작업일 보다 작업일 수가 작거나 같을 때까지는 처음 작업일의 배포일에 함께 배포하도록 작업을 카운트 해서 +1을 해두고, 

처음 작업일 보다 작업일 수가 커지면 그때는 새로운 배포이므로 카운트를 초기화 한 후 커진 작업일 수를 기준으로 뒤의 작업들을 비교하도록 하였습니다. 

```python 
import math
from collections import deque


def solution(progresses, speeds):
    answer = []
    due = deque()
    progresses = deque(progresses)
    for i in range(len(progresses)):
        due.append(math.ceil((100 - progresses[i]) / speeds[i]))
    print(due)

    work = due.popleft()
    cnt = 1
    while due:
        if work >= due[0]:
            cnt = cnt +1
            due.popleft()
        elif work < due[0]:
            answer.append(cnt)
            print(work, cnt)

            work = due.popleft()
            cnt = 1
    answer.append(cnt)
    print (work,cnt)
    return answer

```