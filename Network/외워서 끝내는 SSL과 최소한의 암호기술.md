## 체크섬과 해시

- 체크섬과 해쉬는 무결성을 보장해준다.
- 해시 함수는 충돌을 최소화 해야 하지만, 체크섬 함수는 충돌이 발생하더라고 상관없다.
- 체크섬은 데이터 무결성 검사와 오류감지에만 사용되고, 해시는 데이터 무결성 검사 뿐 만 아니라 고유성 검증 및 다양한 분야에서 사용한다.

### 해시의 특징

- 단방향성
- 입력 값의 크기와 상관없이 결과 값의 길이 가 일정
- 데이터 무결성 확보와 관련해 IT기술 전반에 사용된다.

### 대표적인 hash 알고리즘

- md-5
    - 패스워드 단방향 암호화에 사용금지
- sha -1
- sha-128, 256, 384, 512

### 대표적인 Hash 기술 활용예

- 무결성확보
    - 인증서 검증
    - 디지털 포렌식
    - 디지털 서명(hash + pki)
- 패스워드 단방향 암호화
- 블록체인

---

## 대칭키 비대칭키

### 대칭키

- 키 하나로 암호화/복호화를 모두 수행하는 방식
- 대칭키 알고리즘은 비대칭키 보다 효율이 안좋다
    - 어떤 키 알고리즘을 사용해도 상관없다면 대칭 키 알고리즘을 사용하는게 맞다.
- des, 3des, seed-128, ARIA, AES-128, AES-256 알고리즘이 유명하다.

![image](https://user-images.githubusercontent.com/70142711/234302109-9106a26e-2b80-4909-9d2c-a6650e8c6fd4.png)

대칭키는 내부적으로 결국 ***XOR연산***으로 동작한다고 한다.(전부는 아닌듯)

### 비대칭키

- 한 쌍의 키가 서로 상호 작용하는 구조를 갖는다.
- 두 키 중 하나로 암호화 하면 쌍을 이루는 다른 키로 복호화 한다.
- 보통 Public key와 Private Key로 구분하며 PKI(Public Key Infrastructure) 기술의 근간을 이룬다.
- RSA-2048 ECC 알고리즘이 있다.

![image](https://user-images.githubusercontent.com/70142711/234302225-71593dc3-a4d2-491c-8f65-b08df78c82cd.png)

ex)65가 평문이고 5가 KEY라고 했을때, 65를 5제곱한다.

그 후 나누기 값(Modulus)인 323으로 나누면 12라는 암호화된 데이터가 된다.

- 복호화 할 땐 반대로
- KEY와 Modulus가 될 수 있는 숫자는 소수여야 한다.
- key와 Modulus 의 값만 노출된다.
    - 위의 예시에선 5와 323을 합쳐서 공개키라고 한다.

![image](https://user-images.githubusercontent.com/70142711/234302294-519bc4d9-238a-4e1c-b9c5-f0ffd6ed33bb.png)

비대칭키 보다 대칭키의 효율이 좋다.

- AES128 vs RSA 2048
    - 키 길이가 비대칭키인 rsa2048이 훨씬 더 길어서 복잡하고 연산을 많이 하지만, 보안성이 AES128이 더 뛰어나거나 엇비슷하다고 함.

### 디지털 서명이란?

![image](https://user-images.githubusercontent.com/70142711/234302345-3882fd5a-50ff-4189-8933-1180d4b11638.png)

서명은

- 위.변조 방지
- 부인방지 (실제로 계약했는데 안했다고 거짓말하는)

위 두 가지를 방지해야 한다.

인증서 안에는 Public key가 들어있고 나만 가지고 있는 Private Key가 있다.

원본 내용의 해시 결과를 private Key로 암호화 한다. 

데이터가 위변조 되었는지 확인하기 위해서 해시를 확인하려면 public key로 풀어서 비교해야 한다.

문서를 줄때 해시를 검증할 수 있는 public key와 문서를 같이 준다. 

**private key로 hash를 암호화 하는게 디지털 서명의 실체이다.**

---

## PKI 시스템과 인터넷

### 인터넷을 위한 비대칭키 체계

![image](https://user-images.githubusercontent.com/70142711/234302414-6f49f6a2-d9ab-49b0-a262-71d228f4b908.png)

대칭키는 암호화, 복호화에 전부사용된다. 

대칭키를 상대 편에게 100%안전하게 전달하는 방법은 없다.

인터넷상에서 부적합하다고 판단하여 나온게 비대칭키 

![image](https://user-images.githubusercontent.com/70142711/234302490-d63abeca-ca6d-4dd5-940b-4c361187d835.png)

1. 클라이언트와 서버는 각 키의 쌍을 만든다.

![image](https://user-images.githubusercontent.com/70142711/234302546-70b6cb62-9dc9-4b40-bfdf-949e5a2c681a.png)

1. 클라이언트와 서버는 인터넷망을 통해 자신의 public key를 교환한다.

1. 
    1. **클라이언트에서 서버로 데이터를 보낼 때**
        
        클라이언트는 서버가 보내준 public key를 이용하여 보낼 데이터를 암호화 한다.
        
    2. **반대로 서버가 클라이언트에 데이터를 보낼 때**
        
        서버는 클라이언트가 보내준 public key 를 이용하여 보낼 데이터를 암호화 한다.
        

1. 암호화한 데이터는 TCP/IP위에 만들어진 터널을 통해 전달된다. 
2. 전달받은 데이터는 자신의 private key로 복호화 하여 사용한다.

- 1번의 키 쌍을 생성하는 작업이 시간이 굉장히 오래 걸린다.
- 인터넷망의 public key를 해커가 알게되면 public key를 가지고 private key를 어느정도 유추할 수 있다.(유추하기 쉽지 않게 하기위해, 비대칭키는 키길이가 길다.)

---

### 효율 극대화를 위한 혼합 활용

- 인터넷 환경에서 비대칭키는 안 써도 된다면 안쓰는게 좋다.
- 대칭키와 비대칭키를 혼합 사용하여 효율을 극대화 할 수 있다.

![image](https://user-images.githubusercontent.com/70142711/234302617-d3cc9082-79f1-4178-a564-6535cae23d20.png)

1. 서버는 private key 와 public key 쌍인 비대칭키를 생성한다.
2. 클라이언트는 대칭키를 생성한다.
3. 서버는 본인의 public key 를 클라이언트한테 보낸다.
4. 클라이언트는 전달 받은 서버의 public key로 본인의 대칭키를 암호화 한다. 
5. 클라이언트는 암호화한 본인의 대칭키를 서버에게 전달한다.
6. 서버는 본인의 개인키로 전달받은 대칭키를 복호화 한다.

인터넷 망에서 대칭 키가 암호화 되서 전송 되기 떄문에 해킹 당하지않는 이상 키를 알아낼 수 없다.

![image](https://user-images.githubusercontent.com/70142711/234302688-8054f4c0-eb9e-4c21-a5ba-749d65c823cb.png)

대칭키가 효율이 좋기 때문에 문서를 전달할 떄 클라이언트의 대칭키로 암호화를 한다.

그런데 서버도 위의 flow를 거쳤으면,  클라이언트의 대칭키를 가지고 있기 때문에 복호화 할 수 있다.

- PC의 대칭키를 **SESSION KEY**라고도 한다.
    - SESSION KEY는 통신하는 주체 마다, 생성되기 떄문에, 보안에 우수하다.
- 대칭키든, 비대칭키든 키 쌍을 생성하는 일은 부하가 되는 작업이다.

---

### 비대칭키 체계의 문제점

![image](https://user-images.githubusercontent.com/70142711/234302924-b70d20e6-7b1f-41cc-8469-3043d332ab62.png)

혼합 사용을 위해 클라이언트는 서버의 public key를 받아야 한다. 

근데 이 public key 가 서버의 public key라고 확신 할수 없다.

- 해커들은 MITM이라는 중간자 공격으로 가짜 트래픽을 전달 할 수 있다.

![image](https://user-images.githubusercontent.com/70142711/234303088-75169776-9b49-4bd7-943d-05da69acb752.png)

해커도 중간에서 똑같이 public-private key쌍을 만들어 가짜 key를 전달할 수 있다.

- 가짜 키쌍을 통해 데이터를 탈취할 수 있음.
    1. 해커가 서버의 public key를 가로채고 클라이언트에게 해커의 public key를 전달한다.
    2. 클라이언트는 해커의 public key로 본인의 대칭키를 암호화후 전송한다.
    3. 해커는 해커의 private키로 대칭키를 복호화하여 대칭키를 확보 하여 데이터를 볼 수 있다.
    4. 그 후 아까 가로챈 서버의 public key로 암호화해서 서버에게 전달한다. 
    - 서버와 클라이언트는 탈취당한다는 사실을 모른채 통신하게 된다.

**클라이언트는 서버가 보낸 key가 맞는지 반드시 검증을 해야 한다.**

---

### 공개키 신뢰를 위한 검증체계

이전 강의의 Process의 기초되기 떄문에 다시 한번 읽고 넘어오자.

![image](https://user-images.githubusercontent.com/70142711/234303176-b0ce71d8-f023-446d-8560-0b57d0ce8365.png)

키 쌍을 생성하는 것은 비용이 매우 크기 떄문에, 1회 생성한다.(일정 기간동안 사용)

![image](https://user-images.githubusercontent.com/70142711/234303222-9ce77751-9803-4e7d-a65f-d864d11755ad.png)

실무자는 RA라는 기관에 키쌍 구매를 요청한다. [인증서]

- 인증서에 따라 가격은 천차만별이다.
- RA는 등록대행 기관이다.

구매요청을 받은 RA는 CA에게 요청을 전달하면 CA가 인증서를 생성해서 RA에게 전달한다.

- 인증서 생성은 RA또는 CA둘 중에 하나의 기관이 생성한다고 한다.(기본적으로는 CA가 생성)
- X.509라는 공개 키 인증서(Public Key Certificate) 국제 표준에 맞춰 Public Key에 무결성을 위한 해쉬값과 기타 부가정보를 추가한게 인증서다.
- CA는 Public Key가 포함된 인증서와 Private Key를 RA에게 전달 한다.

RA가 실무자에게 인증서와 Private Key를 전달한다.

- 실무자가 웹서버에 인증서와 Private Key를 설치한다.
    - HTTP에 사용되는 인증서기 떄문에 SSL인증서라고 한다.
    - 인증서의 유효기간은 보통 6개월~1년이다.

![image](https://user-images.githubusercontent.com/70142711/234303289-accf842b-9795-43d7-8453-b7abaf5da2ba.png)

클라이언트가 웹서버에 요청하면 이전 강의 처럼 public key가 날아오는게 아니라, public key가 포함된 인증서가 날아온다.

웹 서버와 클라이언트는 CA라는 인증기관을 신뢰한다.

- CA의 대표 기관중 하나는 베리사인이 있다.

만약 클라이언트가 window 10 OS 를 사용한다고 가정한다면, MS사는 베리사인 업체와 제휴가 맺어져 있고, 정기적인 OS 업데이트를 통해 PC에 **기관 인증서를 설치**한다.

- 이 기관 인증서로 웹서버에서 날아온 public key를 포함한 인증서를 검증하는 것이다.

어떻게 웹서버에서 날아온 인증서를 검증할 수있을까?

- 인증서에는 해당 데이터의 무결성을 위한 해쉬 값이 포함되어 있다.

![image](https://user-images.githubusercontent.com/70142711/234303371-bdc94ca2-8d69-4efb-b259-7651071dca35.png)

CA도 private / public key 쌍을 가지고 있는데, CA는 서버의 인증서를 만들어 줄 때 인증서의 해쉬값을 private key로 암호화 한다.

- 여기서 기관 인증서는 CA의 Public key이다.

결론은 public key를 포함한 인증서의 해쉬 값은 CA의 Private Key로 암호화 되어있기 떄문에 해쉬값을 보기 위해 기관 인증서의 Public Key로 복호화해서 해쉬값을 봐서 검증한다.

- 기관인증서는 os update를 통해 주기적으로 갱신된다.

검증이 완료되면 마찬가지로 전달받은 public key로 클라이언트의 대칭키(Session Key)를 암호화 해서 서버에 전달한다.

---

![image](https://user-images.githubusercontent.com/70142711/234303452-4fe37436-7b33-410f-b6e0-06b39a257818.png)

![image](https://user-images.githubusercontent.com/70142711/234303508-0aad007c-85b4-4d0c-afbc-4e115f674f5b.png)

브라우저에서 위와 같은 x.509형식의 인증서를 확인 할 수 있다.

![image](https://user-images.githubusercontent.com/70142711/234303565-35c1dfff-e65d-4a24-bf78-9d8d8a9fc969.png)

요약하면 인증서는 위와 항목을 가진다.(X.509)

- 유저 정보
    - 인증서 누가 샀는지.. 등등
- Public Key
- CA의 Private Key로 암호화된 Hash

![image](https://user-images.githubusercontent.com/70142711/234303623-52a0603c-520e-43c6-8db4-259fd47061ee.png)

certmgr.msc 명령어로 기관의 인증서들을 확인 할 수 있다.

---

정리하면 HTTPS 통신을 시작할 때

TCP/IP 세션을 맺고 그 후에 SSL/TLS 핸드셰이크 를 수행한다.

이 과정에서 이번 글에서 배웠던 인증서 검증, 대칭키 교환등의 작업들이 이루어져 결과적으로 클라이언트의 대칭키가 서버에 전달된다.

그 후 모든 통신은 클라이언트의 대칭키로 암 복호화 하여 암호화 통신을 하는 것이다.

---

![image](https://user-images.githubusercontent.com/70142711/234303730-a9e00953-6c14-4003-bd96-75d61f53365b.png)

- 민간 기업도 CA를 할수있게 되서, 카카오,은행 등의 인증서가 발급 되기 시작했다.

![image](https://user-images.githubusercontent.com/70142711/234303774-46dfadbe-749c-4e14-a514-eb376232b60f.png)

CA/ RA의 상위기관이 존재한다.
