---
title: 하위 계층 (L1~L3) 프로토콜 및 기술
weight: 2
---

OSI 7계층 모델의 물리 계층(L1), 데이터 링크 계층(L2), 네트워크 계층(L3)에 해당하는 프로토콜과 기술을 다룬다. 이 계층들은 실제 데이터 전송 매체 접근, 프레임 단위 오류 제어, 논리 주소 기반 경로 탐색까지의 핵심 인프라 기능을 담당한다.

## 학습 토픽

| 번호 | 토픽명 | 계층 | 핵심 키워드 |
|---|---|---|---|
| 1 | [L1/L2 MAC 및 에러·흐름 제어](l1-l2-mac-error) | L1, L2 | CSMA/CD, CSMA/CA, ARQ, 슬라이딩 윈도우 |
| 2 | [L2 스위치·VLAN·STP](l2-switch-vlan-stp) | L2 | MAC 학습, 802.1Q 트렁킹, Spanning Tree |
| 3 | [L3 IP 주소 체계 (IPv4/IPv6/NAT)](l3-ip-addressing) | L3 | CIDR, 서브네팅, IPv6 전환, NAT/PAT |
| 4 | [L3 라우팅 프로토콜](l3-routing) | L3 | RIP, OSPF, BGP, ARP, ICMP, IGMP |

## 계층별 주요 기능 요약

| 계층 | 이름 | PDU | 주요 기능 | 대표 프로토콜 |
|---|---|---|---|---|
| L1 | 물리 계층 | 비트 | 신호 전송, 매체 접근 제어 | 이더넷 물리, DSL, Wi-Fi 물리 |
| L2 | 데이터 링크 계층 | 프레임 | MAC 주소 기반 전달, 오류 검출·수정 | Ethernet, VLAN(802.1Q), STP(802.1D) |
| L3 | 네트워크 계층 | 패킷 | 논리 주소 지정, 경로 결정, 라우팅 | IP, ARP, ICMP, OSPF, BGP |
