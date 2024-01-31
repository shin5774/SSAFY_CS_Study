# OS

## OS

> 프로세스와 스레드의 차이를 설명해보세요.
>

## 개념적 차이

### 프로세스

- 운영체제는 컴퓨터(CPU,Memory)의 자원에 대한 분배 권한을 가지고 있다. 그리고 정상적인 작동의 수행을 위해 해당 자원을 할당해야 한다.
    - 프로세스 : OS로부터 자원을 할당받고 해당 **자원에 대한 소유권을 가지는 단위**
- 프로세스는 서로 독립적인 메모리 공간을 가지며 서로의 데이터에 직접 접근할 수 없다.
    - 즉 프로세스는 각각 자체 주소 공간과 자원을 보유하고 있다.

### 쓰레드

- 운영체제가 할당한 자원을 이용하여 프로세스는 작업을 수행할 것이다.
    - 프로세스의 자원을 이용하여 **작업을 수행하는 실행 흐름의 단위**
- 쓰레드들은 속한 프로세스의 자원을 공유한다.

## 상태 (생명 주기)적 차이

### 프로세스

> 5 - 상태 모델에 따르면 프로세스는 5가지 상태를 가지고 있다.
>

| 생성 (new) | 준비 (ready) | 수행 (running) | 블록 / 대기 | 종료 (terminated) |
| --- | --- | --- | --- | --- |

![Untitled](https://private-user-images.githubusercontent.com/84346055/301166806-88d0594d-3012-469b-af25-59b34eaa5806.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDY3MDYzNjMsIm5iZiI6MTcwNjcwNjA2MywicGF0aCI6Ii84NDM0NjA1NS8zMDExNjY4MDYtODhkMDU5NGQtMzAxMi00NjliLWFmMjUtNTliMzRlYWE1ODA2LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAxMzElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMTMxVDEzMDEwM1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTU3YjFlOTE5MDM5OGM5ZTFiZDg5OTk1ODY3OGYwZDZiMjRlMmJmZTkyMDBmZWU0ZTRmOGMyMWM0NDlhNzM3YzEmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.8YMuK11u24aI-TKPik0bk2k7JE1cr4S1amr-Bo7T7VU)

- **생성**
    - 생성되었지만 OS에 의해 수행 가능한 프로세스 풀에 진입하지 않은 상태 (주기억장치에 적재되지 않은 상태)
- **준비**
- **수행**
    - 현재 수행 중인 프로세스
- **블록 / 대기**
    - I / O 작업 등 외부의 작업에 의해 이벤트가 발생할 때까지 수행될 수 없는 프로세스
- 종료
    - OS에 의해 수행 가능 프로세스 풀에서 방출된 프로세스

### 쓰레드

> 프로세스처럼 쓰레드도 수행 상태를 가진다. 하지만 상태에 대한 정의에 차이가 존재한다.
>

![image1](https://private-user-images.githubusercontent.com/84346055/301166822-bf7a4dbc-9a48-4698-ab5f-24883f3aa69c.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDY3MDYzNjMsIm5iZiI6MTcwNjcwNjA2MywicGF0aCI6Ii84NDM0NjA1NS8zMDExNjY4MjItYmY3YTRkYmMtOWE0OC00Njk4LWFiNWYtMjQ4ODNmM2FhNjljLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAxMzElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMTMxVDEzMDEwM1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTVjMjg3ZTg5YzBmOTM5MWM0Njg0MGM4NWE5NTM2YTI1OTBjZjRlMzQzZWU0MjRkZGFjODI2ZWE3MzgwMmRjNmImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.AUsIzr5GtXZgVJ-D12Kgy7RyltTCt32VyMQz3xtPbtg)
| 생성 (Spawn) | 블록 (Block) | 비블록 (Unblock) | 종료 (terminated) |
| --- | --- | --- | --- |
- 생성
    - 일반적으로 새로운 프로세스가 생성될 경우 이 프로세스를 위한 쓰레드도 함께 생성된다.
- 블록
    - 쓰레드가 특정 이벤트를 기다려야 할 때 대기 상태에 돌입한다.
- 비블록
    - 블록 상태가 해제되어 CPU를 할당 받을 수 있는 상태

### 생성과 소멸에 대하여

- 프로세스
  - 프로세스의 생성과 소멸은 큰 오버헤드가 발생한다. 오버헤드가 큰 이유로는 새로운 메모리 공간을 할당하고 초기화하는 작업 때문이다.
- 쓰레드
  - 쓰레드는 프로세스와 다르게 메모리 공간에 대한 제약이 덜하다. 즉 같은 프로세스의 자원을 공유하기 때문에 생성과 소멸에 대한 오버헤드가 적다. 

## 메모리 공유 차이


![image2](https://private-user-images.githubusercontent.com/84346055/301166826-76418790-44c2-4fe5-9c8c-67639319702a.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDY3MDYzNjMsIm5iZiI6MTcwNjcwNjA2MywicGF0aCI6Ii84NDM0NjA1NS8zMDExNjY4MjYtNzY0MTg3OTAtNDRjMi00ZmU1LTljOGMtNjc2MzkzMTk3MDJhLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAxMzElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMTMxVDEzMDEwM1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTIyZTZmYjgyNTNjOWU5NzY3MmUyNjc1MTc3OGU5ZDViNGFiMmJhOWQyMWJkOTY4ZTE5NTcxZTk4MmU4NjdjZmQmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.hrR4j2zqf2EwA2Jyd29DBVqhZZSEgLqkuDtXQkdbnfg)
### 프로세스
- 프로세스는 독립된 메모리 공간을 가지고 있어, 프로세스 간에 직접적인 데이터 공유가 어렵다. 프로세스 간 통신(IPC) 메커니즘을 사용하여 데이터를 교환 가능.
### 쓰레드
- 쓰레드는 같은 프로세스 내에 속해 있으므로, 프로세스 내의 자원을 공유할 수 있다. 이는 데이터 및 자원에 대한 효율적인 공유를 가능.
