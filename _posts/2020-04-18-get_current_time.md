---
layout: post
title: "[Python] 현재 날짜와 시간 가져오기"
date: 2020-04-18
tags:
    - python
---

```python
from datetime import datetime

now = datetime.now() # Return type: datetime.datetime

now.strftime('%Y-%m-%d %H:%M:%S') # Return type: str
```
> '2020-04-18 17:28:30'

### Timezone 설정해서 가져오기

```python
from datetime import datetime
import pytz

tz_Seoul = pytz.timezone('Asia/Seoul') # Return type: pytz.tzfile.Asia/Seoul
now = datetime.now(tz_Seoul)

now.strftime('%Y-%m-%d %H:%M:%S')
```
> '2020-04-18 17:46:31'

### References

- [Python Get Current time](https://www.programiz.com/python-programming/datetime/current-time)
