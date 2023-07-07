---
title: "[python] Pandas DataFrame"
excerpt: "데이터 전처리를 위한 Pandas DataFrame 기초"
categories:
 - python
tags:
 - python
 - pandas
 - dataframe
 - programming
---

[python pandas 공식 홈페이지로 이동](https://docs.python.org/3/tutorial/datastructures.html)


## DataFrame 생성
기본적으로 파일명, 인코딩 방식, 헤더를 파라미터 값으로 넣어주면 된다.<br>
헤더는 컬럼명이 있는 행을 지정한다. pandas의 경우 첫번째 행을 0으로 표시하므로 첫번째 행에 컬럼명이 표시된 경우 헤더를 0으로 지정하면 된다. 만약 컬럼이 없다면 직접 추가하거나 None으로 지정할 수 있다.

```python
import pandas as pd
# encoding='utf-8-sig'/'CP949'...,
# header=number of row/None
# columns=['name1', 'name2', 'name3']
df = pd.DataFrame('abc.csv', encoding='utf-8-sig', header=0) 

# list 활용
lst = [[1, 2, 3], [2, 3, 4], [3, 4, 5]]
df = pd.DataFrame(lst, columns=['a', 'b', 'c'])

# dictionary 활용
dic_lst = [{'a': 0, 'b': 1, 'c': 2},{'a': 3, 'b': 4, 'c': 5},{'a': 6, 'b': 7, 'c': 8}]
df = pd.DataFrame(dic_lst)

# dictionary의 key가 섞여있어도 key와 column을 매칭해서 df 생성
# key에 존재하지 않는 칼럼은 nan으로 자동 처리
df = pd.DataFrame(['a', 'b', 'c'])
dic_lst = [{'c': 0, 'b': 1, 'a': 2}, {'a': 3, 'b': 4, 'c': 5}, {'b': 6, 'a': 7, 'c': 8}]
df = df.append(dic_lst)
```

## column 순서 바꾸기

```python
# 칼럼명 이용하는 경우
# 칼럼이 a, b, c 인 경우, b, c, a 로 순서 변경
df = [['b', 'c', 'a']]

# 리스트 활용하는 경우
columns = df.columns.to_list()
columns1 = columns[:3]
columns2 = columns[3:]
columns_reorder = columns2 + columns1
df = df[columns]
```

## DataFrame 필터링
- pandas에서 str을 다루기 위해 series로 접근해야 한다.
- boolean(True, False)으로 다룰 수 있다.

```python
# df['name'] == 'jason'이 True/False로 반환되고, True인 row만 출력
df[df['name'] == 'jason'] 

# df['name']이 null인 row만 출력
df[df['name'].isnull()] 

# df['name']이 [a, b, c] 원소에 속하는 경우만 출력, 검색할 값이 1개더라도 isin() 파라미터는 list로 들어가야 함.
df[df['name'].isin(['a', 'b', 'c'])] 

# &, |로 and, or 조건을 적용할 수 있음
# ()로 구분해줘야 제대로 인식함.
df[(df['name'] == 'jason') | (df['name'] == 'leo')]
df[(df['name'] == 'jason') & (df['smoking'] == 'no')]
```