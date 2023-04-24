- NAT 는 기본적으로 PtoP(peer to peer)통신을 방해한다.

## 1. Symmetric NAT

- Symmetric NAT은 특정 PC의 내부의 IP:Port가 외부 IP:Port로 변환될 때 Destination에 따라 `다른 외부 Port`가 사용된다.
- 패킷을 보내는 외부 서버마다 다른 NAT 매핑을 사용한다.
- PC에서 패킷을 특정 서버로 보내면 그 서버에서 보낸 패킷만 PC로 전달된다.
- Restricted cone NAT은 공유기 외부 포트가 항상 일정하지만 Symmetric NAT은 접속하는 서버에 따라 외부 포트가 달라진다.
- 보안성이 뛰어 나다.
- TCP 세션 마다 외부 포트 지정
- Symmetric NAT의 NAT table
    
   ![image](https://user-images.githubusercontent.com/70142711/234000830-b35674cc-84f9-489a-a276-26a85e8f967e.png)
    
- outbound시 NAT table NAT정보가 추가 된다.
- inbound시 NAT table을 해당 정보가 존재하면 통신 된다.
- 포트번호를 어느 정도 예측할 수도 있지만, 보안성을 위해 포트 번호를 랜덤하게 하여 보안성을 높일 수 잇다
    - 그러나 이렇게 하면 p2p통신을 하기는 더 힘들어 진다.
        - p2p통신은 게임 뿐만아니라 WebRTC라는 기술도 사용
            - 코로나로 인해 화상이 각광 받는데, 이런 데이터는 서버를 거치면 트래픽이 많기 때문에 최대한 P2P통신을 하는게 좋다.
    - 그러나 이미 이러한 단점을 극복하기 위한 기술이 나와있음.
        - 포트포워딩?
    - ***WebRTC***
        
        ***WebRTC***는 서버를 최대한 거치지 않고 P2P(Peer-to-Peer Network)로 브라우저나 단말 간에 데이터를 주고받는 기술의 웹 표준입니다.
        
    

# 2. Cone NAT

- Host 단위로 외부 포트 지정
- 특정 PC의 내부의 IP:Port가 외부 IP:Port로 변환될 때 Destination에 상관 없이 `외부 Port가 항상 일정하다`.

## 2-1. Full Cone 방식

- 내부 PC1에서 변환된 IP/PORT로 outbound 트래픽이 발생하면, 외부에서 해당 IP/PORT로 들어오는 모든 패킷은 PC1로 전달 된다.
- NAT Table에 추가 시 Remote IP와 Remote Port을 제한하지 않는다.
    - 보안성이 떨어 지지만, 게임사 같은경우 사용 한다고 함.

![image](https://user-images.githubusercontent.com/70142711/234000958-8c998fbf-a0e0-442e-981f-09a6b3f296e1.png)

- 192.168.0.10:3000이 Web server A와 통신을 한번 하여 NAT table에 저장되었다면
    
    Web server B도 192.168.0.10:3000로 통신할 수 있게 된다. 
    

## 2-2. (IP) Restricted Cone 방식

- Full Cone 방식에서 IP 만 최초 NAT table 등록 시 Remote IP를 제한 하는 방법
- 일반 적으로 Restricted Cone은 ip로 제한하는 방식이다.
- Full Cone 방식처럼 NAT table에 등록 되었다고 어느 서버나 접근 할 수 있는게 아닌, Remote IP로 등록 서버만 접근 가능.
- Full Cone보다 보안성이 높다.
- (IP) Restricted Cone NAT Table EX)
    
    ![image](https://user-images.githubusercontent.com/70142711/234001019-a7f679a1-0a84-432a-a8fc-105fea9af466.png)
    

## 2-3. Port Restricted Cone 방식

- (IP) Restricted Cone 방식처럼 주소만 보는게 아닌, 주소와 포트 모두를 보는 방식
- 자주 사용하진 않는다고 함
- NAT TABLE EX)
    
    ![image](https://user-images.githubusercontent.com/70142711/234001065-49aad4d7-024c-45ef-a0a1-443117bdd633.png)
