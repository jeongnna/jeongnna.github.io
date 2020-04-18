---
layout: post
title: "[Python] Get Current Time Using datetime Module"
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

### Current time of a timezone

```python
from datetime import datetime
import pytz

tz_Seoul = pytz.timezone('Asia/Seoul') # Return type: pytz.tzfile.Asia/Seoul
now = datetime.now(tz_Seoul)

now.strftime('%Y-%m-%d %H:%M:%S')
```
> '2020-04-18 17:46:31'
