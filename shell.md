# 시스템 프로그래밍

## 쉘 - 다중 사용자 지원 관련 명령어

---

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

## 쉘 - 파일 및 권한 관련 명령어

---

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