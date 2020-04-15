---
layout: post
title: "셸 스크립트 프로그래밍"
date: 2018-9-25
---
# 7장. 셸 스크립트 프로그래밍

## CentOS의 bash 셸

- 기본 셸은 bash
- bash 셸의 특징
    + Alias 기능(명령어 단축 기능)
    + History 기능(위/아래 화살표키
    + 연산 기능
    + Job Control 기능
    + 자동 이름 완성 기능(탭키)
    + 프롬프트 제어 기능
    + 명령 편집 기능
- 셸의 명령문 처리 방법
    + `명령어 [옵션...] [인자...]`
    + 예) `rm -rf /mydir`

## 환경변수

- `echo $환경변수이름`으로 확인 가능
- `export 환경변수=값`으로 환경 변수의 값을 변경
- 주요 환경변수

환경변수 | 설명 | 환경변수 | 설명
--- | --- | --- | ---
HOME | 현재 사용자의 홈 디렉토리 | PATH | 실행 파일을 찾는 디렉토리 경로
LANG | 기본 지원되는 언어 | PWD | 사용자의 현재 작업 디렉토리
TERM | 로그인 터미널 타입 | SHELL | 로그인해서 사용하는 셸
USER | 현재 사용자의 이름 | DISPLAY | X 디스플레이 이름
COLUMNS | 현재 터미널의 컬럼 수 | LINES | 현재 터미널 라인 수
PS1 | 1차 명령 프롬프트 변수 | PS2 | 2차 명령 프롬프트(대개는 ')')
BASH | bash 셸의 경로 | BASH_VERSION | bash 버전
HISTFILE | 히스토리 파일의 경로 | HISTSIZE | 히스토리 파일에 저장되는 개수
HOSTNAME | 호스트의 이름 | USERNAME | 현재 사용자 이름
LOGNAME | 로그인 이름 | LS_COLORS | 'ls" 명령어의 확장자 색상 옵션
MAIL | 메일을 보관하는 경로 | OSTYPE | 운영체제 타입

## 셸 스크립트 프로그래밍

- C언어와 유사하게 프로그래밍이 가능
- 변수, 반복문, 제어문 등의 사용이 가능
- 별도로 컴파일하지 않고 텍스트 파일 형태로 바로 실행
- vi나 gedit으로 작성이 가능
- 리눅스의 많은 부분이 셸 스크립트로 작성되어 있음

## 셸 스크립트의 작성과 실행

- vi나 gedit으로 작성
- 실행 방법
    1. `sh <스크립트 파일>` 명령으로 실행
    2. `chmod +x <스크립트 파일>` 명령으로 실행 가능 속성으로 변경한 후에 `./<스크립트 파일>` 명령으로 실행

`name.sh`:
```bash
#!/bin/sh
echo "사용자 이름: " $USERNAME
echo "호스트 이름: " $HOSTNAME
exit 0
```

```bash
sh name.sh

chmod u+x name.sh
./name.sh
```

## 변수의 기본

- 변수는 사용하기 전에 미리 선언하지 않으며, 변수에 처음 값이 할당되면서 자동으로 변수가 생성
- 모든 변수는 문자열(String)로 취급
- 변수 이름은 대소문자를 구분
- 변수를 대입할 때 `=` 좌우에는 공백이 없어야 함

```bash
testval = Hello # Error
testval=Hello
echo $testval
testval=Yes Sir # Error
testval="Yes Sir"
echo $testval
testval=7+5
echo $testval # "7+5"
```

## 변수의 입력과 출력

- `$` 문자가 들어간 글자를 출력하려면 `''`로 묶어주거나 앞에 `\`를 붙임
- `""`로 변수를 묶어줘도 된다.

```bash
myvar="Hi Woo"
echo $myvar # "Hi Woo"
echo "$myvar" # "Hi Woo"
echo '$myvar' # "$myvar"
echo \$myvar # "$myvar"
echo 값 입력: # "값 입력:"
read myvar # "안녕하세요?"
echo '$myvar' = $myvar # "$myvar = 안녕하세요?"
```

## 숫자 계산

- 변수에 대입된 값은 모두 문자열로 취급
- 변수에 들어있는 값을 숫자로 해서 `+`, `-`, `*`, `/` 등의 연산을 하려면 `expr`을 사용
- 수식에 괄호 또는 `*`는 그 앞에 꼭 `\`를 붙임

```bash
num1=100
num2=$num1+200
echo $num2# "100+200"
num3='expr $num1 + 200'
echo $num3
num4='expr \($num1 + 200 \) / 10 \* 2'
echo $num4
```

## 파라미터 변수

- 파라미터 변수는 `$0, $1, $2, ...`의 형태를 가짐
- 전체 파라미터는 `$*`로 표현

명령어 | yum | -y | install | gftp
--- | --- | --- | --- | ---
파라미터 변수 | $0 | $1 | $2 | $3

`paravar.sh`:
```bash
`#!/bin/sh`
`echo "실행파일 이름은 <$0>이다"`
`echo "첫 번째 파라미터는 <$1>이고, 두 번째 파라미터는 <$2>이다"`
`echo "전체 파라미터는 <$*>이다"`
`exit 0`
```

```bash
sh paravar.sh 값1 값2 값3
# "실행파일 이름은 <paravar.sh>이다"
# "첫 번째 파라미터는 <값1>이고, 두 번째 파라미터는 <값2>이다"
# 전체 파라미터는 <값1 값2 값3>이다"
```

## 기본 `if` 문

형식:
```bash
if [ 조건 ] # 띄어쓰기 반드시 필요
then
    참일 경우 실행
fi
```

```bash
#!/bin/sh
if [ "woo"="woo" ]
then
    echo "참입니다"
fi
exit 0
```

## `if`~`else` 문

형식:
```bash
if [ 조건 ]
then
    참일 경우 실행
else
    거짓일 경우 실행
fi
```

```bash
#!/bin/sh
if [ "woo"!="woo" ]
then
    echo "참입니다"
else
    echo "거짓입니다"
fi
exit 0
```

## 조건문에 들어가는 비교 연산자

문자열 비교 | 결과
--- | ---
"문자열1"="문자열2" | 두 문자열이 같으면 참
"문자열1"!="문자열2" | 두 문자열이 같지 않으면 참
-n "문자열" | 문자열이 NULL이 아니면 참
-z "문자열" | 문자열이 NULL이면 참

산술 비교 | 결과
--- | ---
수식1 -eq 수식2 | 두 수식(또는 변수)이 같으면 참
수식1 -ne 수식2 | 두 수식(또는 변수)이 같지 않으면 참
수식1 -gt 수식2 | 수식1이 크다면 참
수식1 -ge 수식2 | 수식1이 크거나 같으면 참
수식1 -lt 수식2 | 수식1이 작으면 참
수식1 -le 수식2 | 수식1이 작거나 같으면 참
!수식 | 수식이 거짓이면 참

```bash
#!/bin/sh
if [ 100 -eq 200 ]
then
    echo "100과 200은 같다."
else
    echo "100과 200은 다르다."
fi
exit 0
```

## 파일과 관련된 조건

조건 | 결과
--- | ---
-d 파일명 | 파일이 디렉토리면 참
-e 파일명 | 파일이 존재하면 참
-f 파일명 | 파일이 일반 파일이면 참
-g 파일명 | 파일이 set-group-id가 설정되면 참
-r 파일명 | 파일이 읽기 가능이면 참
-s 파일명 | 파일 크기가 0이 아니면 참
-u 파일명 | 파일에 set-user-id가 설정되면 참
-w 파일명 | 파일이 쓰기 가능이면 참
-x 파일명 | 파일이 실행 가능이면 참

```bash
#!/bin/sh
fname=/lib/systemd/system/httpd.service
if [ -f $fname ]
then
    head -5 $fname
else
    echo "웹 서버가 설치되지 않았습니다."
fi
exit 0
```

## `case`~`esac` 문

- `if` 문은 참과 거짓의 두 경우만 사용(2중분기)
- 여러 가지 경우의 수가 있다면 `case` 문(다중분기)

`case1.sh`:
```bash
#!/bin/sh
case "$1" in
    start)
        echo "시작~~";;
    stop)
        echo "중지~~";;
    restart)  
        echo "다시 시작~~";;  
    *)  
        echo "뭔지 모름~~";;  
esac  
exit 0
```

```bash
sh case1.sh stop $ "중지~~"
```

`case2.sh`:
```bash
#!/bin/sh
echo "리눅스가 재미있나요? (yes / no)"
read answer
case $answer in
    yes | y | Y | Yes | YES)
        echo "다행입니다."
        echo "더욱 열심히 하세요 ^^";;
    [nN]*)
        echo "안타깝네요. ㅠㅠ";;
    *)
        echo "yes 아니면 no만 입력했어야죠."
        exit 1;;
esac
exit 0
```

```bash
sh case2.sh
```

## AND, OR 관계 연산자

- and는 `-a` 또는 `&&` 사용
- or는 `-o` 또는 `||` tkdyd

```bash
#!/bin/sh
echo "보고 싶은 파일명을 입력하세요."
read fname
if [ -f $fname] && [ -s $fname ]
then
    head -5 $fname
else
    echo "파일이 없거나 크기가 0입니다."
fi
exit 0
```

# 반복문 `for` 문

형식:
```bash
for 변수 in 값1 값2 값3 ...
do
    반복할 문장
done
```

```bash
#!/bin/sh
hap=0
for i in 1 2 3 4 5 6 7 8 9 10
do
    hap='expr $hap + $i'
done
echo "1부터 10까지의 합: "$hap
exit 0
```

3행은 `for((i=1; i<=10; i++))` 또는 `for i in 'seq 1 10'`으로 대체 가능

현재 디렉토리에 있는 셸 스크립트 파일(`*.sh`)의 파일명과 앞 3줄을 출력하는 프로그램
```bash
#!/bin/sh
for fname in $(ls *.sh)
do
    echo "-----$fname-----"
    head -3 $fname
done
exit 0
```

## 반복문 `while` 문

조건식이 참인 동안 계속 반복 (탈출: `Ctrl+c`)

```bash
#!/bin/sh
while [ 1 ]
do
    echo "CentOS 7"
done
exit 0
```

1에서 10까지의 합계를 출력하는 프로그램
```bash
#!/bin/sh
hap=0
i=1
while [ $i -le 10 ]
do
    hap='exr $hap + $i'
    i='expr $i + 1'
done
echo "1부터 10까지의 합: "$hap
exit 0
```

비밀번호를 입력받고 비밀번호가 맞을 때까지 계속 입력을 받는 프로그램
```bash
#!/bin/sh
echo "비밀번호를 입력하세요."
read mypass
while [ $mypass != "1234" ]
do
    echo "틀렸음. 다시 입력하세요."
    read mypass
done
echo "통과~~"
exit 0
```

## `until` 문

- `while` 문과 용도가 거의 같지만, `until` 문은 조건식이 거짓인 동안 계속 반복한다.
- 위의 `while` 문 예시를 동일한 용도로 `until` 문으로 바꾸려면 4행을 `until [ $i -gt 10 ]`으로 바꾸면 된다.

## `break`, `continue`, `exit`, `return` 문

- `break`는 주로 반복문을 종료할 때 사용되며, `continue`는 반복문의 조건식으로 돌아가게 함. `exit`는 해당 프로그램을 완전히 종료함. `return`은 함수 안에서 사용될 수 있으며 함수를 호출한 곳으로 돌아가게 함.

```bash
#!/bin/sh
echo "무한반복 입력을 시작합니다(b: break, c:continue, e: exit)"
while [ 1 ]
do
    read input
    case $input in
        b|B)
            break;;
        c|C)
            echo "continue를 누르면 while의 조건으로 돌아감"
            continue;;
        e|E)
            echo "exit를 누르면 프로그램(함수)를 완전히 종료함"
            exit 1;;
esac;
done
echo "break를 누르면 while을 빠져나와 지금 이 문장이 출력됨"
exit 0
```

## 사용자 정의 함수

형식:
```bash
함수이름 () {
    내용들...
}
```

```bash
#!/bin/sh
myFunction () {
    echo "함수 안으로 들어왔음"
    return
}
echo "프로그램을 시작합니다."
myFunction
echo "프로그램을 종료합니다."
exit 0
```

## 함수의 파라미터 사용

형식:
```bash
함수이름 () {
    $1, $2, ... 등을 사용
}
```

```bash
#!/bin/sh
hap () {
    echo 'expr $1 + $2'
}
echo "10 더하기 20을 실행합니다."
hap 10 20
exit 0
```

## `eval`

- 문자열을 명령문으로 인식하고 실행

```bash
#!/bin/sh
str="ls -al"
echo $str
eval $str
exit 0
```

## `export`

- 외부 변수로 선언해준다. 즉, 선언한 변수를 다른 프로그램에서도 사용할 수 있도록 해줌

`exp1.sh`:
```bash
#!/bin/sh
echo $var1
echo $var2
exit 0
```

`exp2.sh`
```bash
#!/bin/sh
var1="지역 변수"
export var2="외부 변수"
sh exp1.sh
exit 0
```

## `printf`

- C언어의 `printf()` 함수와 비슷하게 형식을 지정해서 출력

```bash
#!/bin/sh
var1=100.5
var2="재미있는 리눅스~~"
printf "%5.2f \n\n \t %s \n" $var1 "$var2"
exit
```

## `set`과 `$` 명령어

- 리눅스 명령어를 결과로 사용하기 위해서는 `$(명령어)` 형식을 사용
- 결과를 파라미터로 사용하고자 할 때는 `set`과 함께 사용

```bash
echo "오늘 날짜는 $(date) 입니다."
set $(date)
echo "오늘은 $4 요일 입니다."
exit 0
```

## `shift`

- 파라미터 변수를 왼쪽으로 한 단계씩 아래로 쉬프트시킴
- 10개가 넘는 파라미터 변수에 접근할 때 사용
- 단, $0 파라미터 변수는 변경되지 않음

원하지 않는 결과의 소스:
```bash
#!/bin/sh
myfunc () {
    echo $1 $2 $3 $4 $5 $6 $7 $8 $9 $10 $11
}

myfunc AAA BBB CCC DDD EEE FFF GGG HHH III JJJ KKK
exit 0
```

shift 사용을 통해 위 문제를 해결
```bash
#!/bin/sh
myfunc (0 {
    str=""
    while [ "$1"!="" ]
    do
        str="$str $1"
        shift
    done
    echo $str
}

myfunc AAA BBB CCC DDD EEE FFF GGG HHH III JJJ KKK
exit 0
```
