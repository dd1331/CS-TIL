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
# 쉘 - foreground/background 프로세스 관리/ 제어

## 프로세스 vs 바이너리

- 코드 이미지 또는 바이너리: 실행파일
- 실행중인 프로그램: 프로세스
    - 가상 메모리 및 물리 메모리 정보
    - 시스템 리소스 관련 정보
    - 스케쥴링 단위

## 리눅스는 다양한 프로세스 실행 환경

- 유닉스 철학: 여러 프로그램이 서로 유기적으로 각자의 일을 수행하면서 전체 시스템이 동작하도록 하는 모델

## foreground process / background process

- foreground process: 쉘에서 해당 프로세스 실행을 명령한 후, 해당 프로세스 수행 종료까지 사용자가 다른 입력을 하지 못하는 프로세스
    - ctr + z 실행 중지 상태로 변경 (맨 마지막 중지된 프로세스는 bg 명령으로 백그라운드 프로세스로 실행될 수 있음)
    - jobs: 백그라운드 진행 또는 중지된 상태의 프로세스 노출
- background process: 사용자 입력관 상관없이 실행되는 프로세스
    - 쉘에서 해당 프로세스 실행시 맨 뒤에 &을 붙여줌
    - 예

        ```bash
        find / -name '*.py' > list.txt *
        ```

## 프로세스 상태 확인 - ps

```bash
# ps [option(s)]
-a : 시스템을 사용하는 모든 사용자의 프로세스 출력 (보통 u, x옵션과 같이 사용)
-u : 프로세스 소유자에 대한 상세 정보 출력
-l : 프로세스 관련 상세 정보 출력
-x : 터미널에 로그인한 후 실행한 프로세스가 아닌 프로세스들도 출력
		 주로 데몬 프로세스 확인용
-e : 해당 프로세스 관련 환경변수도 출력
-f : 프로세스간 관계 정보도 출력
```

```bash
USER : 프로세스 실행시킨 사용자 아이디
PID : 프로세스 아이디
%CPU : 마지막 1분 동안 프로세스가 사용한 cpu 시간의 백분율
%MEM : 마지막 1분 동안 프로세스가 사용한 메모리 백분율
VSZ : 프로세스가 사용하는 가상 메모리 크기
RSS : 프로세스에서 사용하는 실제 메모리 크기
STAT : 프로세스 상태
START : 프로세스 시작시간
TIME : 현재까지 사용된 cpu 시간(분:초)
COMMAND : 명령어
```

## 프로세스 중지 - kill

- kill 작업번호(job number)
- kill 프로세스 아이디(pid)
- 작업 강제 종료 옵션 -9
    - 예

    ```bash
    find / -name '*.py' > list.txt &
    # [1] 57
    kill -9 57
    ```

## 주로 사용하는 프로세스 명령

- ps aux | grep 프로세스명: 프로세스가 실행 중인지 확인하고 관련 프로세스 정보 출력
- kill -9 프로세스 아이디(pid): 프로세스 강제종료
- 명령&: 터미널에서 다른 작업을 하거나 프로세스 실행에 오랜 시간이 걸릴경우 백그라운드로 실행
- ctr + z: 프로세스 중지
- ctr + c: 프로세스 종료(실행취소)

# 쉘 - 리눅스와 파일

- 모든 것은 파일이라는 철학을 따름
    - 모든 인터렉션은 파일을 읽고, 쓰는 것처럼 이루어져있음
    - 마우스, 키보드와 같은 모든 디바이스 관련 기술도 파일과 같이 다루어짐
    - **모든 자원에 대한 추상화 인터페이스로 파일 인터페이스 활용**

## 가상 파일 시스템

- 파일 네임스페이스
    - A 드라이브 (A:/), 드라이브 (C:/windows)와 같은 윈도우 형식 x
    - 전역 네임스페이스 사용
        - /media/floofy/dave.jpg

## 슈퍼블록, inode와 파일

- 슈퍼블록: 파일 시스템의 정보
- 파일: inode 고유값과 자료구조에 의해 주요 정보 관리
    - 'filename:inode'로 파일이름은 inode 번호와 매칭
    - 파일 시스템에서는 inode를 기반으로 파일 액세스
    - inode 기반 메타 데이터 저장

## 리눅스 파일 시스템(ext file system)

- 구성: inode 기반 메타데이터(파일권한, 소유자 정보, 파일 사이즈, 생성시간 등 시간 관련 정보, 데이터 저장 위치 등)

## 파일과 inode

- 리눅스 파일 탐색: 예 - /home/ubuntu/link.txt
    1. 각 디렉토리 엔트리(dentry) 탐색
        - 각 엔트리는 해당 디렉토리 파일/디렉토리 정보를 가지고 있음
    2. '/' dentry에서 'home'을 찾고 'home'에서 'ubuntu'를 찾고, 'ubuntu'에서 link.txt 파일이름에 해당하는 inode 얻음

## 하드링크와 소프트링크

- cp 명령: 파일 복사
    - 10MB 사이즈를 가지고 있는 A파일을 B파일로 복사
        - cp A B - > A와 B는 각각 물리적으로 10MB 파일로 저장
- 하드링크: ln A B
    - A와 B는 동일한 10MB 파일을 가리킴
    - 동일한 파일을 가진 이름을 하나 더 만든 것
        - inode는 동일
        - 전체파일 용량은 달라지지 않음
    - ls -i (파일 inode 확인)
    - ls -al (완전 동일한 파일 확인)
    - rm A로 A를 삭제해도 B는 해당파일 접근 가능
        - A에 해당되는 inode 정보만 삭제되고 inode가 가리키는 실제 파일은 존재
- 소프트(심볼릭) 링크: ln -s A B
    - 윈도우의 바로가기와 동일
    - ls -al로 소프트링크 확인 가능
    - rm A로 A삭제시 B에서도 해당 파일 접근불가
    - inode에서 원본(A) 링크의 inode를 경유하기 때문

- 하드링크/소프트링크 모두 파일 수정시 동일한 내용 접근 가능

# 프로세스

## 프로세스 ID

- pid, 각 프로세스는 해당시점에 유니크한 pid를 가짐
- pid 최대값은 32768
    - 부호형(signed) 16비트 정수값 사용 (2^15 = 32768

## 프로세스 계층

- 최초 프로세스: init 프로세스, pid1
- init 프로세스는 운영체제가 생성
- 다른 프로세스는 또다른 프로세스로부터 생성
    - 부모 프로세스, 자식 프로세스
- ppid: 부모프로세스의 pid

```bash
ps -ef
# -e  시스템상의 모든 프로세스애 대한 정보 출력
# -f  UID, PID, PPID, CPU%, STIME, TTY, TIME, CMD 출력
```

## 프로세스 소유자 관리

- 리눅스 내부에서는 프로세스의 소유자(사용자)와 그룹을 UID/GID (정수)로 관리
- 사용자에 보여줄때에만 UID와 사용자이름 매핑 정보를 기반으로 사용자 이름으로 제공

```bash
ps -ef
sudo vi /etc/passwd
sudo vi/ etc/shadow
```

## 프로세스 관리 관련 시스템콜

### getpid(), getppid()

- 프로세스 아이디와 부모프로세스 아이디 반환

```bash
#include <sys/type.h>
#include <inistd.h>
pid_t getpid (void);
pid_t getppid (void);
```

## 프로세스 기본 구조

- TEXT, DATA, BSS, HEAP, STACK

## 프로세스 생성

- 기본 프로세스 생성 과정
    1. TEXT, DATA, BSS, HEAP, STACK의 공간을 생성
    2. 프로세스 이미지를 해당 공간에 업로드하고 실행 시작
- 프로세스 계층: 다른 프로세스는 또다른 프로세스로부터 생성
    - 부모 프로세스, 자식 프로세스

## fork(), exec() 시스템 콜

- fork()
    - 새로운 프로세스 공간을 별도로 만들고 호출한 프로세스(부모 프로세스) 공간을 모두 복사
        - 별도의 프로세스 공간을 만들고 부모 프로세스 공간의 데이터를 그대로 복사
    - 자식프로세스에서는 pid 0 리턴 부모 프로세스에서는 실제 pid 리턴
    - 자식과 부모프로세스의 변수 및 PC(Program Count) 값은 동일
    - 새로운 프로세스 공간을 별도로 만들고 fork() 시스템 콜을 호출한 프로세스 공간을 모두 복사 후 fork() 코드 이후 코드 부터 실행
- exec()
    - 호출한 현재 프로세스 공간의 TEXT, DATA, BSS 영역을 새로운 프로세스의 이미지로 덮어씌움
        - 별도의 프로세스 공간을 만들지 않음