---
layout: post
title: "[Python] Pandas DataFrame Indexing"
date: 2000-01-01
tags:
    - python
---

## 열 인덱싱

```python
df['year'] # return 값: Series
df.loc[:, 'year']
```

```python
df[['year', 'month']] # return 값: DataFrame
df.loc[:, ['year', 'month']]
```

## 열 대입

열의 모든 성분에 동일한 값 대입
```python
df['penalty'] = 0.5
```

열 대입
```python
df['penalty'] = [0.1, 0.2, 0.3, 0.4, 0.5]
```

열 생성과 동시에 대입
```python
df['zeros'] = np.arrange(5)
```

열에 Series를 대입하면 index가 맞춰져서 들어감

열 삭제
```python
del df['zeros']
```

## 행 인덱싱

```python
df[0:3]
df['two':'four']
```

```python
df.loc['two']
df.loc['two':'four']
df.loc[df['year'] > 2014]
```

```python
df.iloc[1:3]
```

## 행 대입

```python
df.loc['seven'] = [2017, 'cho', 4.0, 1]
```

## 행/열 동시 인덱싱

```python
df.loc['two':'four', ['year', 'month']]
df.iloc[1:3, 1:3]
```
