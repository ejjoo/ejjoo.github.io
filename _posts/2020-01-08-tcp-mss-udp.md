---
layout: post
title: TCP의 MTU, MSS 그리고 UDP
date: 2020-01-09 19:12:47 +0900
categories: network
---
# MTU, MSS?
MTU는 Maximum Transmission Unit, MSS는 Maximum Segment Size의 약자다. TCP에서 사용할 수 있는 사용자 데이터의 최대 크기를 나타내는 말로, 일반적인 Ethernet(V2) 환경에서 MTU는 1500바이트, MSS는 TCP의 헤더와 IP헤더를 제외한 1460바이트의 크기 제한을 가진다. (타임스탬프 옵션 제외시)

![](/public/images/2020-01-08-tcp-mss-udp-1.png)

둘의 목적은 네트워크 오버헤드를 줄이고 단일 패킷이 네트워크를 독점하는것을 방지하기 위해서이다. MTU 크기는 네트워크 오버헤드를 고려해서 설정한다. MTU를 크게 잡으면 더 큰 유저 데이터를 전송할 수 있게 되므로 효율적으로 보일수도 있지만, 그만큼 단위시간당 처리가능한 패킷의 수가 줄어들게 되어 네트워크 딜레이의 원인이 된다. 또한 오류제어를 하지 않는 패킷이 로스가 발생할 경우 그만큼 재전송해야하는 패킷이 커지는 문제도 있다. 점보프레임을 사용하는 경우에 MTU크기는 9Kb까지 사용가능하지만, 라우팅 과정에 지원하지 않는 장비가 있는경우 사용할 수없다.

# UDP는 패킷크기의 제한이 없다?
일반적으로 UDP(User Datagram Protocol)는 한 패킷을 하나의 개체로 보고, TCP은 패킷 전체를 스트림으로 보기 때문에 UDP는 별도로 MSS를 정의하지 않는다. 하지만 네트워크 레이어에서 일반적으로 패킷을 분할해 전송하기 때문에 사실은 분할되어 전송된다고 봐야한다. 물론 점보프레임을 지원하는 장비내에서는 얘기가 달라지지만, 미리 예측할수 있는 문제는 아니다. 

![](/public/images/2020-01-08-tcp-mss-udp-2.png)

그리고 패킷 헤더의 길이 저장공간이 2바이트이기 때문에 사실 패킷의 크기는 2의 16승인 65535에서 헤더사이즈를 제외한 65,507byte로 제한된다. UDP를 사용하더라도 대부분의 라우터들이 MTU 사이즈로 세그먼트를 하는경우가 많기 때문에, 가능하면 1500바이트 단위로 패킷을 전송하는게 효율적이다.

##### 참고자료
---
<http://www.faqs.org/rfcs/rfc791.html>
<https://searchnetworking.techtarget.com/definition/maximum-transmission-unit><br>
<https://www.imperva.com/blog/mtu-mss-explained/><br>
<https://serverfault.com/questions/246508/how-is-the-mtu-is-65535-in-udp-but-ethernet-does-not-allow-frame-size-more-than><br>
<http://nenunena.tistory.com/60><br>
<http://www.gpgstudy.com/forum/viewtopic.php?t=6131><br>
<https://en.wikipedia.org/wiki/User_Datagram_Protocol><br>