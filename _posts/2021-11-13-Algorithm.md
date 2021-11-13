---
title: 2021-11-13 hash table
tags: [Python,Algorithm,hash]
categories: Algorithm
---
# Hash table(python dictionary)
오늘은 자료 구조 중 hash 에 대해 공부했습니다. dictionary는 python을 사용하면서 정말 많이 사용했던 자료 구조인데, 
이제서야 각잡고 배우게 되었습니다. 

## Hash 구조
### 기본 
해쉬 구조는 Key 와 Value 가 한 쌍으로 이루어져있는 자료 구조 입니다. 
앞서 Java 시간에 배웠던 것 처럼 인덱스가 없어서 순서 보장은 되지 않지만 원하는 것을 빠르게 찾을 수 있다는 장점이 있습니다. 

Key는 중복이 허용되지 않아요. Key로 value값을 얻어내야 하니까요. 하지만 value는 어떤 것이든 올 수 있습니다. 중복도 가능하고, 문자열 , 리스트 , 심지어 dictionary 도 들어갈 수 있어요.

#### 좀 더 깊게 들어가보자. 
python 의 dictionary는 곧바로 hashtable을 제공하긴 하지만, 
더 깊게 들어가봅니다. 
key 로 들어간 값인 문자열은 해시 함수를 거쳐서 숫자로 변환 됩니다. 
그리고 이 값이 주소 값이 되서 value를 저장하게 되는 것이지요. 
Java 시간에 배웠던 Heap 영역과 stack 영역이 나누어지는 그것 인것 같습니다. 
요약 하면 문자열 그자체가 주소 값이 되고 이 주소 값은 value 가 있는 곳이 되니, 
나중에 문자열로 검색 하면 주소 값으로 변환해서 그 주소를 가져오는 것이 되겠네요. 

그러면  문자열 이 달라도 주소값이 같아지는 일은 없을까요? 

있네요. 충돌이 발생하기도 한다고 합니다. 
- 문제 해결법 
  - 체이닝 기법 : 두 키가 같은 hashkey 를 가지면 hash key 의 해시 테이블을 두고, 링크드 리스트를 만들어서 key와 value 를 저장 하게 합니다. 
    - 그러면 나중에 key -> hash key 로 해시 테이블을 찾아서 다시 key 로 value를 찾아올 수 가 있어요. 
  - 선형 조사 기법 :  헤시 테이블의 빈 곳에 값을 넣어줍니다. 그 해시 테이블안에서 해결을 보려는 건데,
    - 알고리즘은 다음과 같은가봐요. 
      - 먼저 입력 하려는 key를 hash key 로 바꾼 값이 table에 존재 하는지 확인합니다. 
      - 만약 존재 한다면,현재 테이블에서  값을 저장할 수 있는 마지막 hash 번호(?) 를 찾아요. 그리고 여기에 key 와 value를 저장합니다. 
      - 저장된 값을 찾아 올때는 
        - 먼저 key 값으로 변환된 hash key 로 우선 찾아보고,
        - 이 값이 존재 하지않을 경우에는 빈공간에 존재 하는지 찾아보기 위해서 위에 저장한 로직과 똑같은 규칙으로 hash의 테이블마다 key 값을 불러내서 찾아옵니다. 
        

그러니까, 충돌 문제가 없을 경우에는 키 = 주소값 이니 한번에 값을 찾아올 수 있는데, 
충돌이 발생하면 모두 뒤져 봐야하겠네요. 

##python dictionary 사용법 

### 생성하기 
- dictionary 를 선언(?) 한 후에 사용 할 수 도 있고, (d = {'a': 1, 'b': 2, 'c':3 )
- 빈 dictionary 를 만든 후에 key 와 value를 저장해가면서 사용할 수 있습니다. (d['a'] = 1 )

# 모두의 알고리즘 예제 풀이 
- 딕셔너리를 이용해 동명이인을 찾는 알고리즘 | p.140
```python
# 두 번 이상 나온 이름 찾기
# 입력: 이름이 n개 들어 있는 리스트
# 출력: n개의 이름 중 반복되는 이름의 집합
# 1번을 해보세요!
def find_same_name(s):
name_dict = {}
result = set()
for i in s :
if i in name_dict.keys() :
name_dict[i] += 1
else:
name_dict[i] = 1
for j in name_dict.keys():
if name_dict[j] > 1:
result.add(j)
return result

```
- 모든 친구를 찾는 알고리즘 | p.150

```python
def print_all_friends(g,start):
    qu = []
    done = set()
    qu.append(start)
    done.add(start)
    while qu : 
        p = qu.pop(0)
        print(p)
        for i in g[p]:
            if i not in done: 
                qu.append(i)
                done.add(i)
                
```       

- 친밀도 계산 알고리즘 
```python
def print_all_friends(g, start):
qu = []
done = []
qu.append((start,0))
done.append(start)

    while qu:
        (p, c) = qu.pop(0)
        print(p, c)

        for i in g[p]:
            if i not in done:
                qu.append((i, c + 1))
                done.append(i)

```