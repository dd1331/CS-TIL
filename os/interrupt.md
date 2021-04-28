# 인터럽트
- CPU가 프로그램을 실행하고 있을 때, 입출력 하드웨어 등의 장치나 예외상황이 발생하여 처리가 필요할 경우에 CPU에 알려서 처리하는 기술 (ex.선점형 스케쥴러에서 프로세스 상태 변경하는 것)

## 인터럽트 종류

- 내부 인터럽트 (소프트웨어 인터럽트)
    - 주로 프로그램 내부에서 잘못된 명력 또는 잘못된 데이터 사용시 발생
        - **0으로 나눴을 때**
        - 사용자 모드에서 허용되지 않은 명령 또는 공간 접근시
        - 계산결과가 오버플로우/언더플로우 날 때
- 외부 인터럽트 (하드웨어 인터럽트)
    - 주로 하드웨어에서 발생되는 이벤트 (프로그램 외부)
        - 전원이상
        - 기계문제
        - **키보드등 IO 관련 이벤트**
        - **Timer 이벤트**

## 시스템 콜 인터럽트

- 시스템 콜 실행을 위해서는 강제로 코드에 인터럽트 명령을 넣어 CPU에게 실행시켜야 한다.
- 시스템 콜 실제 코드
    - eax 레지스터에 시스템 콜 번호를 넣는다
    - ebx 레지스터에 시스템 콜에 해당하는 인자값을 넣는다
    - 소프트웨어 인터럽트 명령을 호출하면서 0x80 값을 넘겨준다

```bash
mov, eax, 1
mov, ebx, 0
**int** 0x80 
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1fc1d2d5-22cf-4536-961f-5837726f0afc/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1fc1d2d5-22cf-4536-961f-5837726f0afc/Untitled.png)

## 인터럽트와 IDT

- 인터럽트는 미리 정의되어 각각 번호와 실행코드를 가리키는 주소가 기록되어 있음
    - IDT(Interrupt Description Table)에 기록
    - 컴퓨터 부팅시 운영체제가 기록
    - 운영체제 내부 코드