- Apache-Jmeter 5.5 기준 
- java로 만들어 졌기 떄문에, Jdk깔려 있어야함
- Apache Jmeter 사이트에서 zip다운로드
- 압출 풀고 bin안에 Apache Jmeter.jar 실행 

---
<br>

### 테스트 계획 오른쪽클릭 - 추가 - 스레드들 -  setUp 스레드 그룹 
![image](https://user-images.githubusercontent.com/70142711/174576286-88cd5397-b289-4e89-a706-b38c9f19ec9a.png)
- 쓰레드들의 수  : user수 
- Ramp-up시간 :Thread(Virtual User) 생성에 걸리는 시간
- 루프 카운트 : 각 user마다 요청 횟수
> Ex1) 쓰레드수 10 , 루프카운트 10이라면 10명이 동시에 10회 요청 하는것

> Ex2)  쓰레드수 10, Ramp-up 10이라면, 10초에 걸쳐서 스레드 10개를 생성해라, 즉 1총 스레드 1개씩 생성 함

<br>

### setUp 쓰레드 그룹 오른쪽 클릭 - 추가 - 표본 추출기 - HTTP 요청 (웹으로 접속 할 경우)
![image](https://user-images.githubusercontent.com/70142711/174576468-48ba5826-d868-4c01-b552-3e811acc062c.png)
- 도메인, 경로, 포트번호 ,파라미터 등을 셋팅

![image](https://user-images.githubusercontent.com/70142711/174576522-7cf779b6-46c1-4d88-89e2-7e8a7bb8c351.png)
- 초록색 실행 버튼누르면 실행 된다.

![image](https://user-images.githubusercontent.com/70142711/174576713-8215a256-4cf1-4307-8c24-40a74219b676.png)
- 위와같이 한 스레드 그룹에 여러 요청을 셋팅 할 수 있다.
- 그럼 한 쓰레드 그룹이 동작할떄 여러요청을 동시에 보내게 된다. 
- 요청이 10 번이면 10번씩 x 요청 2개인지 10번을 나눠서 요청하는지는 테스트 필요 
> 테스트 결과 : 2유저 10회요청이면 추가한 HTTP요청마다 20번씩 요청 함 

<br>

### 여기까지는 요청을 보내긴 하지만, 결과를 알수가 없다. 
- 통계로 요청에 대한 결과를 확인하자.

![image](https://user-images.githubusercontent.com/70142711/174576815-22ab1908-e593-4079-83fd-3eb193193d0c.png)
1. HTTP요청에대한 통계를 보려면 HTTP요청에서 - 오른쪽 버튼 클릭후 ADD
2. 쓰레드그룹안에 모든 접속에대한 통계를 보고싶으면 쓰레드 그룹 에서 - 오른쪽 버튼 클릭후 ADD
3. 테스트 전체에 대한 모든 통계를 보고 싶으면 테스트 계획 - 오른쪽 버튼 클릭후 ADD

- 여기서는 HTTP요청에 대한 통계를 볼것이다. 
- HTTP요청 - 오른쪽클릭 - 추가 - 리스너 - 결과들의 트리 보기
> EX) 5번 요청하면, 각 요청에 대한 결과와 리턴값을 볼 수 있다.

<br>

![image](https://user-images.githubusercontent.com/70142711/174576895-e9011c46-3542-4220-9517-ac91f41a731c.png)
- 마찬가지로 오른쪽 위의 실행버튼 클릭

<br>

![image](https://user-images.githubusercontent.com/70142711/174577004-d37b5527-f3f0-47a4-917d-66ac570c5433.png)
- 위와같이 요청에 대한 결과와 응답값 받아볼 수있다.

<br>

### ※.HTTP 헤더값 세팅
![image](https://user-images.githubusercontent.com/70142711/174577095-e1cf6408-7253-46de-a284-3727f04ae10a.png)
- HTTP 헤더 관리자를 추가해서 헤더값 세팅을 해줘야함. 
- application/json형태로 보내기위한 설정

<br>

**참고**
- https://story.stevenlab.io/207

![image](https://user-images.githubusercontent.com/70142711/174577360-26a3ee92-f354-4e30-8084-7041e6db0138.png)
해당 글을 참고하면

- 요청부터 응답 시작전까지가 레이턴시
- 요청부터 응답이끝날때까지가 로드 타임
> 일반적으로 로드타임으로 성능체크를 많이 한다고 한다.

<br>

![image](https://user-images.githubusercontent.com/70142711/174577447-879ddf45-6dc2-4d4d-9887-94770bc84c8e.png)
- 결과를 보면 로드타임, 레이턴시가 같음. (순식간에끝나서 체크할수없다?라고 영상에서말함)
- 밀리세컨드 기준이다.
  - 로드타임이 17이기때문에 0.017초걸린것

- 근데 이건 하나의 요청에 대한 정보가 담겨 있어 전체적인 통찰을 얻기 어렵다.

<br>

![image](https://user-images.githubusercontent.com/70142711/174577607-e30c46ae-64b1-483b-970b-04c72b6233d0.png)
- HTTP요청 - 오른쪽 클릭 - 추가 - 리스너 - Summary Report

![image](https://user-images.githubusercontent.com/70142711/174577730-2cd9a6ad-1d38-4cf6-b124-d796ec7c92a6.png)
- 마찬가지로 실행은 상단 초록색 바
- 전체 요청에 대한 요약 리포트를 볼 수 있다.
  - Samples 는 요청 횟수
  - Average는 걸린 평균 시간 
  - Min은 요청의 최소 시간
  - Throughput는 초당 몇번 요청 했는지 
    - 높을 수록 좋음. 
  - Std. Dev. 는 표준편차 0에가까울수록 요청들이 평균 시간과 가깝고, 높을 수록 들쑥날쑥하다는 말

<br>
### 시각적으로 보고 싶을때
- HTTP요청 - 오른쪽 클릭 - 추가 - 리스너 -Graph Results 클릭

![image](https://user-images.githubusercontent.com/70142711/174577934-8f2e1132-ef05-4de5-bc21-055278273b5b.png)
![image](https://user-images.githubusercontent.com/70142711/174577939-5d18d782-0bd3-4218-9dec-d08d3c7923d1.png)
- 마찬가지로 실행하여, 몇초가 걸렸는지 평균, 표준편차,등등을 시각적으로 볼 수 있음.




