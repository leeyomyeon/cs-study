3-way Handshake에서 교환되는 추가 정보와 정책

1. SYN 패킷 (클라이언트 → 서버)
   클라이언트는 연결 요청과 함께 초기 설정 및 옵션 정보를 전달합니다:

MSS (Maximum Segment Size):
클라이언트가 한 번에 전송할 수 있는 TCP 세그먼트의 최대 크기를 제안합니다.
이는 IP 패킷 크기(기본적으로 MTU)에서 TCP/IP 헤더 크기를 뺀 값입니다.
예: Ethernet의 MTU가 1500바이트라면 MSS는 일반적으로 1460바이트.
Window Size:
클라이언트가 수신 가능한 데이터의 크기를 나타냅니다.
이를 통해 흐름 제어(flow control)를 지원합니다.
Selective Acknowledgment (SACK) Permitted:
선택적 ACK 옵션 지원 여부를 표시합니다.
이 옵션이 활성화되면, 패킷 손실 시 효율적으로 재전송할 수 있습니다.
Timestamps (옵션):
RTT(Round Trip Time) 측정 및 PAWS(Protect Against Wrapped Sequence)와 같은 기능 지원. 2. SYN-ACK 패킷 (서버 → 클라이언트)
서버는 클라이언트 요청을 수락하며 자신만의 설정과 옵션 정보를 추가로 전달합니다:

MSS: 서버도 자신의 MSS 값을 제안합니다.
Window Size: 서버가 수신 가능한 데이터 크기를 설정합니다.
SACK Permitted: 서버 측에서도 SACK 옵션을 사용할 수 있음을 알립니다.
Timestamps: 서버가 지원하는 경우, 해당 값을 추가로 포함합니다. 3. ACK 패킷 (클라이언트 → 서버)
ACK 패킷은 연결을 최종적으로 확정하며, 클라이언트와 서버 간 정책 및 옵션 설정이 완료됩니다.
이 단계에서는 추가적인 설정은 포함되지 않고, 주로 ACK 정보만 전달됩니다.
교환되는 주요 정책 요약
옵션/정책 설명
MSS 한 번에 전송 가능한 최대 세그먼트 크기
Window Size 흐름 제어를 위한 수신 창 크기
SACK Permitted 선택적 ACK를 통한 효율적인 재전송 지원 여부
Timestamps RTT 측정 및 PAWS를 위한 시간 정보
Nagle Algorithm 소량의 데이터를 합쳐 전송할지 여부 (일반적으로 handshake 단계에서 협상되지 않지만 중요한 TCP 정책 중 하나)
정리
TCP 3-way handshake 과정은 단순히 연결을 설정하는 것뿐만 아니라, 전송 효율성과 신뢰성을 높이기 위한 옵션 및 정책을 교환하는 단계입니다. 이를 통해 클라이언트와 서버는 최적의 통신 환경을 협상하게 됩니다.
