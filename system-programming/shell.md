# **시스템 프로그래밍**

# 쉘 - 다중 사용자 지원 관련 명령어

## 쉘 종류

- 쉘(shell): 사용자와 컴퓨터 하드웨어 또는 운영체제간 인터페이스
    - 사용자의 명령을 해석해서, 커널에 명령을 요청해주는 역할
    - 관련된 시스템콜을 사용해서 프로그래밍 되어있다
- Bourne-Again Shell (bash): 대부분의 리눅스 디폴트
- Bourne-Shell (sh)
- C Shell (csh)
- Korn Shell (ksh): 유닉스에서 가장 많이 사용됨

## whoami: 로그인한 사용자 ID 확인

```bash
# whoami
root
```

## passwd: 로그인한 사용자 암호 변경

## useradd: 사용자 기본 설정 없이 유저 생성

## adduser: 사용자 기본 설정으로 유저 생성

## su: 사용자 변경

- 보통 su - 와 함께 사용
    - su root: 현재 사용자의 환경설정 기반, root로 변경
    - su -root: 변경되는 사용자의 환경설정 기반, root로 변경

## sudo: root 권한으로 실행

- root 계정으로 로그인하지 않은 상태에서 root 권한이 필요한 명령을 실행할 수 있도록 하는 프로그램
- /etc/sudoers 설정 파일에서 다음과 같이 설정 변경 가능
    - visudo가 설치되어 있다면 해당 명령을 통해 설정 파일이 오픈되어 바로 수정 가능

```bash
1. 특정 사용자가 sudo를 사용할 수 있도록 설정
		userid ALL=(ALL) ALL
2. 특정 그룹에 포함된 모든 사용자가 sudo를 사용할 수 있도록 설정
		%group ALL=(ALL) ALL
3. 패스워드 생략 설정
		%group ALL=(ALL) NOPASSWD: ALL
		userid ALL=(ALL) NOPASSWD: ALL
```

# 쉘 - 파일 및 권한 관련 명령어

## pwd: 현재 디렉토리 위치

## cd: 디렉토리 이동

- cd - 직전 경로 이동

## ls: 파일 목록 출력

- 와일드카드 사용 가능
    - *는 임의 문자열
    - ?는 문자 하나

```bash
# ls tes* -al
-rwxr-xr-x 1 root root 120 Jul 19 19:28 test-start
-rw------- 1 root root 317 Oct 3 09:47 tests.cnf
```

## man: 메뉴얼 (ex: man <command>)

## 파일 권한

- 파일마다 소유자, 소유자 그룹, 모든 사용자에 대해
    - 읽고, 쓰고, 실행하는 권한 설정
    - 소유자 접근 권한정보는 inode에 저장
- 제일 앞 문자 -와 d는 파일인지 디렉토리인지에 따라 정해진다
- 사용자
    - 소유자: 소유자에 대한 권한
    - 그룹: 소유자가 속해 있는 그룹에 대한 권한
    - 공개: 모든 사용자들에 대한 권한
- 퍼미션 종류
    - 읽기(r), 쓰기(w), 실행(x)

## chmod: 파일 권한 변경

- 기호 사용법
    - 누구: u (user), g (group), o (others), a (all)
    - 어떻게: +, -, =(설정)
    - 무엇을: r, w, x

```bash
# 문자사용
chmod g+rx test.c
chmod u+rw test.c
chmod ug+rw test.c
chmod u=rwx,g=rw,o=rx test.c

# 숫자사용
# r = 2^2 = 4
# w = 2^1 = 2
# x = 2^0 = 1
rwxrwxrwx = 777
r-xr-xr-x = 555
r-------- = 400
rwx------ = 700

chmod 400 mysequrity.pem
```

- 주로 사용하는 옵션
    - chmod -R 777 directory

## chown: 소유자 변경

- chown [option] [owner:group] [file]

```bash
chown root:root file
chown root: file
chown :root file
```

- 주로 사용하는 옵션
    - chown -R root:root directory
- 소유자 그룹 변경
    - chgrp [option] [group] [file]
    - chgrp -R root directory

## cat: 파일 보기

## head: 파일 시작 부분 보기(기본 10줄)

## tail: 파일 끝 부분 보기(기본 10줄)

## more: 파일보기 (화면이 넘어갈 경우 화면이 넘어가기 전까지 보여줌)

# 쉘 - redirection/pipe

## Standard Stream (표준 입출력)

- 명령으로 실행되는 프로세스는 세 가지 스트림
    - 표준 입력 스트림 (Standard Input Stream) -stdin
    - 표준 출력 스트림 (Standard Input Stream) -stdout
    - 오류 출력 스트림 (Standard Input Stream) -stderr
- 모든 스트림은 일반적인 텍스트로 콘솔에 출력하도록 되어있음

## redirection

- 표준 스트림 흐름을 바꿀 수 있다.
    - >, <을 사용
    - 주로 명령어 표준 출력을 화면이 아닌 파일에 쓸 때 ex) 프로그램 > 파일, 파일 > 프로그램
- 예
    1. ls > files.txt
        - ls로 출력되는 표준 출력  스트림의 방향을 files.txt로 바꿔줌
    2. head < files.txt
        - files.txt의 파일 내용이 head라는 파일의 처음부터 10라인까지 출력해주는 명령으로 넣어짐
    3. head < files.txt > files2.txt
        - files.txt의 내용이 head로 들어가서 files.txt의 앞 10 라인출력
        - head의 출력 스트림은 다시 files2.txt로 들어감
        - head는 files.txt 내용을 출력하지 않고, 해당 출력 내용이 다시 files2.txt에 저장됨
    4. 기존 파일에 추가는 >> 또는 << 사용
        - ls >> files.txt
        - 기존에 있는 files.txt 파일 끝에 ls 출력결과 추가

## pipe

- 두 프로세스 사이에서 한 프로세스의 출력 스트림을 또다른 프로세스의 입력 스트림으로 사용할 때 사용됨
- 예
    1. ls | grep file
        - ls 명령을 통한 출력 내용이 grep 명령의 입력 스트림으로 들어감
        - grep file은 grep 명령의 입력스트림을 검색해서 file 이라는 문구가 들어 있는 입력 내용만 출력

grep [-option] [pattern] [file or directory name]

```bash
<option>
		-i : 영문의 대소문자 구별x
		-v : 패턴을 포함하지 않는 라인 출력
		-n : 각행 선두에 행번호 출력
		-l : 파일명만 출력
		-c : 패턴 일치 라인 개수만 출력
		-r : 하위 디렉토리까지 검색
```

grep -E "go|java|python" files.txt → files.txt라는 파일에서 go, java 또는 python이 있는 모든 행을 출력