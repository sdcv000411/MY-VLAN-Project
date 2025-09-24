이 프로젝트는 Cisco Packet Tracer를 활용하여 VLAN을 구성하고, 서로 다른 VLAN 간 통신을 가능하게 하는 토폴로지를 구축한 예제
서버와 여러 개의 PC가 각각 다른 VLAN에 속해 있으며, 라우터의 서브 인터페이스(ROAS, Router on a Stick)를 통해 VLAN 간 라우팅을 지원함

<네트워크 토폴로지>

서버 (Server-PT) → VLAN 100

PC1 → VLAN 10

PC2 → VLAN 20

PC3 → VLAN 30

스위치(2960-24TT) → VLAN 별 포트 할당

멀티레이어(3650-24PS) → Switch 1,2,3 담당

⚙️ VLAN 및 IP 구성
VLAN 정보
VLAN 번호	이름	할당 장비
VLAN 10	Sales	PC1
VLAN 20	HR	PC2
VLAN 30	Marketing	PC3
VLAN 100	ServerNet	Server
<IP 주소 체계>
장비	인터페이스	VLAN	IP 주소	게이트웨이
PC1	Fa0	10	192.168.10.10	192.168.10.1
PC2	Fa0	20	192.168.20.10	192.168.20.1
PC3	Fa0	30	192.168.30.10	192.168.30.1
Server	Fa0	100	192.168.100.10	192.168.100.1
Router	G0/1.10	10	192.168.10.1	-
Router	G0/1.20	20	192.168.20.1	-
Router	G0/1.30	30	192.168.30.1	-
Router	G0/1.100	100	192.168.100.1	-
<라우터 설정 (ROAS)>
Router> enable
Router# configure terminal
Router(config)# interface gig0/1
Router(config-if)# no shutdown

<VLAN 10>
Router(config)# interface gig0/1.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0

<VLAN 20>
Router(config)# interface gig0/1.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0

<VLAN 30>
Router(config)# interface gig0/1.30
Router(config-subif)# encapsulation dot1Q 30
Router(config-subif)# ip address 192.168.30.1 255.255.255.0

! VLAN 100
Router(config)# interface gig0/1.100
Router(config-subif)# encapsulation dot1Q 100
Router(config-subif)# ip address 192.168.100.1 255.255.255.0

🔧 스위치 설정
Switch> enable
Switch# configure terminal

<VLAN 생성>
Switch(config)# vlan 10
Switch(config-vlan)# name Sales
Switch(config)# vlan 20
Switch(config-vlan)# name HR
Switch(config)# vlan 30
Switch(config-vlan)# name Marketing
Switch(config)# vlan 100
Switch(config-vlan)# name ServerNet

<포트 할당>
Switch(config)# interface fa0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10

Switch(config)# interface fa0/2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 20

Switch(config)# interface fa0/3
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 30

Switch(config)# interface gig0/1
Switch(config-if)# switchport mode trunk

<통신 테스트>

각 PC에서 게이트웨이로 Ping → 정상 통신 확인

ping 192.168.10.1


PC1 ↔ PC2, PC3, Server 간 상호 Ping 테스트

ping 192.168.20.10
ping 192.168.30.10
ping 192.168.100.10


모든 장비 간 통신 가능하면 VLAN 라우팅이 정상적으로 동작.

<파일 구조>
/My-VLAN-Proeject
 ├── topology.png   # 패킷 트레이서 토폴로지 캡처
 ├── VLAN.pkt       # Packet Tracer 프로젝트 파일
 └── README.md      # 설명 문서

<학습 포인트>

VLAN 분할 및 관리 방법

Access / Trunk 포트 설정

Router-on-a-Stick(ROAS) 개념

VLAN 간 라우팅 및 통신 테스트
