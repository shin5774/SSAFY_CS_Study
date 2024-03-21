**[운영체제] CHAPTER 2. 운영체제 구조**

**⏰30min**

!https://private-user-images.githubusercontent.com/93027942/315184356-c1c3fb96-eb67-4a22-ae2d-9d0f64692dd7.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTEwMTM5MjgsIm5iZiI6MTcxMTAxMzYyOCwicGF0aCI6Ii85MzAyNzk0Mi8zMTUxODQzNTYtYzFjM2ZiOTYtZWI2Ny00YTIyLWFlMmQtOWQwZjY0NjkyZGQ3LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAzMjElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMzIxVDA5MzM0OFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTQzMTZiZDRiZDQwZTFjMGY2YjczOGExNDE0MzdkNTM5YThmODQ4YTcwNzZjYjlhMWZmNjkxZWI3MWQ3ZGU0MjkmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.1Lrbjt8XyAx5zywsehiIGcs8h0nTaKx9l7i66lQ4n54

**1. 사용자 인터페이스**

**[그림 2.1]은 운영체제 서비스를 바라보는 관점입니다. (a)에 해당하는 3가지에 대해 간략히 서술해주세요.**

- GUI
- CLI
- 터치 스크린 인터페이스

**2. 시스템 콜**

**2-1. 하나의 파일을 다른 파일로 복사하는 과정(입력 파일 이름 획득 ~ 정상적으로 종료)에서 필요로 하는 (b)의 예시를 3가지만 말해주세요. 구체적인 이름을 기입할 필요는 없습니다.**

- 파일 획득
- 파일 읽기
- 파일 쓰기

**2-2. [문제 2-1] 의 경우, 사용자 입장에서 발생할 수 있는 문제와 대안을 설명해주세요. (hint. 문제에 있는 단어 조합)**

- 불편하다. (자세한 구현 사항을 알 수 없다. - > API 확인 필요)

**3. 링커와 로더**

**디스크에 있는 파일을 CPU에서 실행하는 과정을 설명해주세요. (키워드: 링크, 로더 포함)**

- 소스 파일을 컴파일러가 컴파일하면 오브젝트 파일
- 오브젝트 파일을 링크가 링킹 과정을 거치면 이진 실행 파일
- 이진 실행 파일을 로더가 로딩하면 메모리에 프로그램이 올라가서 CPU가 실행

**4. 운영체제와 응용 프로그램**

**Iphone 15 Pro Black Titanium Owner인 주원이는 새로 산 휴대폰이 너무 좋지만 기존에 쓰던 갤럭시와 너무 달라 적응하는 것이 쉽지 않습니다. 불평불만을 토로하는 주원이에게 응용 프로그램이 운영체제마다 다른 이유를 설명해주세요.**

- 결국 응용 프로그램이라는 것은 운영체제에 시스템 콜을 통해 작업을 수행하는데 운영체제가 근본적으로 다를 경우 세부 수행 명령어가 다르기 때문이다.

**5. 운영체제와 구조**

**다음은 운영체제의 구조를 정리한 표입니다. 표를 완성해주세요.**

!https://private-user-images.githubusercontent.com/93027942/315185287-daa51356-b7ba-445b-aaec-4a35b6f334aa.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTEwMTM5MjgsIm5iZiI6MTcxMTAxMzYyOCwicGF0aCI6Ii85MzAyNzk0Mi8zMTUxODUyODctZGFhNTEzNTYtYjdiYS00NDViLWFhZWMtNGEzNWI2ZjMzNGFhLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAzMjElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMzIxVDA5MzM0OFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWQyNzgzZDZkOWI4ODAyN2FkMmNiYWViNTkwNWMzNjRlYzNjYjI1YTU3NTYzMjdmZDlhM2EwYmNmZjk4YTc4NjEmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.ktzwQmCXwClu4ki_069KjtTS94WHZwGFFc3xjFnAcEc

1. 모놀리식
2. 계층별 구조
3. 마이크로 커널
4. 모듈식
5. 계층별 구조
6. 마이크로 커널
7. 확장이 어렵다
8. 사용자 프로그램의 명령어 수행이 여러 계층을 통과하는 과정에서 오버헤드 발생
9. 메시지 전달과 프로세스 복사 과정에서 오버헤드
10. 계층 + 마이크로 커널
