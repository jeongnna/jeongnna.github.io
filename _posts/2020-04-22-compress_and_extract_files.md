---
layout: post
title: "[Linux] 파일 압축 / 압축 풀기"
date: 2020-04-22
tags:
    - linux
---

### zip

파일 압축

```bash
zip filename.zip file1 file2 file3
```

압축 풀기

```bash
unzip filename.zip
```

### tar

파일 압축

```bash
tar -cvf filename.tar file1 file2 file3
```

압축 풀기

```bash
tar -xvf filename.tar
```

### tar.gz

파일 압축

```bash
tar -zcvf filename.tar.gz file1 file2 file3
```

압축 풀기

```bash
tar -zxvf filename.tar.gz
```
