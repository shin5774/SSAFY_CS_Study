# [6주차] OS 기본 구조

**⏰3~~0~~5min**

!https://private-user-images.githubusercontent.com/93027942/315184356-c1c3fb96-eb67-4a22-ae2d-9d0f64692dd7.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTEwMTM3NzcsIm5iZiI6MTcxMTAxMzQ3NywicGF0aCI6Ii85MzAyNzk0Mi8zMTUxODQzNTYtYzFjM2ZiOTYtZWI2Ny00YTIyLWFlMmQtOWQwZjY0NjkyZGQ3LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAzMjElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMzIxVDA5MzExN1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTFiYTBhMzM5YWMwNTZiZGYzYTJkZDA0NTI0MWY1MWNiYjQ4MTMzMmY5OGI1NjQxYTU0NTMxYjRmZjVjYjliOWMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.bHn6fst9mbHxvKH_AqNbsHxLSD-yo7EsvgEWY2pIPzQ

# 1. 사용자 인터페이스

## [그림 2.1]은 운영체제 서비스를 바라보는 관점입니다. (a)에 해당하는 3가지에 대해 간략히 서술해주세요.

1. CLI - 명령어로 제어할 수 있는 인터페이스
2. GUI - 마우스 등으로 제어할 수 있는 인터페이스
3. 터치 인터페이스 - 스마트폰 등에서 터치를 통해 제어할 수 있는 인터페이스

# 2. 시스템 콜

## 2-1. 하나의 파일을 다른 파일로 복사하는 과정(입력 파일 이름 획득 ~ 정상적으로 종료)에서 필요로 하는 (b)의 예시를 3가지만 말해주세요. 구체적인 이름을 기입할 필요는 없습니다.

> 기존 파일 접근, 새로운 파일 생성, 내용 복사
> 

## 2-2. [문제 2-1] 의 경우, 사용자 입장에서 발생할 수 있는 문제와 대안을 설명해주세요. (hint. 문제에 있는 단어 조합)

> 입력 파일의 이름이 잘못된 경우가 있다. → 파일 경로 확인 필요 .?
> 

# 3. 링커와 로더

## 디스크에 있는 파일을 CPU에서 실행하는 과정을 설명해주세요. (키워드: 링크, 로더 포함)

1. 컴파일러가 해당 파일 컴파일
2. 링커가 오브젝트 파일 생성
3. 로더가 실행 가능한 파일 생성
4. 실행

# 4. 운영체제와 응용 프로그램

## Iphone 15 Pro Black Titanium Owner인 주원이는 새로 산 휴대폰이 너무 좋지만 기존에 쓰던 갤럭시와 너무 달라 적응하는 것이 쉽지 않습니다. 불평불만을 토로하는 주원이에게 응용 프로그램이 운영체제마다 다른 이유를 설명해주세요.

> 운영체제는 각기 다른 언어와 구조로 작성되었기 때문에, 각 운영체제에서 잘 동작하도록 하는 응용프로그램은 다를 수 밖에 없다 .?
> 

# 5. 운영체제와 구조

## 다음은 운영체제의 구조를 정리한 표입니다. 표를 완성해주세요.

!https://private-user-images.githubusercontent.com/93027942/315185287-daa51356-b7ba-445b-aaec-4a35b6f334aa.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTEwMTM3NzcsIm5iZiI6MTcxMTAxMzQ3NywicGF0aCI6Ii85MzAyNzk0Mi8zMTUxODUyODctZGFhNTEzNTYtYjdiYS00NDViLWFhZWMtNGEzNWI2ZjMzNGFhLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAzMjElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMzIxVDA5MzExN1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPThhMzQzNDc3ZTE3NzMxMjRiNTQ4MjJhNzIxMmRhMzM2YmFkMzhhNDcyMzY2NTQ2MTM0N2FjMWJjZDFiYTYxMzkmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.IxTWY_5Dex_cNjDKVC3d9LDUBDJp25DIatc1I2gnD34

1. 모놀리식 접근 방식
2. 계층화된 접근 방식
3. 마이크로커널 방식
4. 모듈식 방식
5. 계층화된 접근 방식
6. 마이크로커널 방식
7. 구현, 확장 어려움
8. 오버헤드 발생 시 성능 저하
9. 메시지 전달 호출 필요
10. 모놀리식
11. 모듈식
12. 하이브리드
