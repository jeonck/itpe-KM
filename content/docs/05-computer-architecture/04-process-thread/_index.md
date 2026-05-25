---
title: 프로세스 및 스레드 관리
weight: 4
---

운영체제의 핵심인 프로세스·스레드 관리와 CPU 스케줄링은 시스템 성능과 자원 효율을 결정하는 기반 기술이다. 본 섹션에서는 프로세스 생명주기와 문맥 교환, 멀티스레딩 모델, 스케줄링 알고리즘 세 가지 핵심 주제를 다룬다.

| 토픽 | 핵심 내용 | 주요 키워드 |
|---|---|---|
| **[프로세스 관리](process)** | PCB 구조, 프로세스 상태 전이(Create→Ready→Running→Waiting→Terminated), 문맥 교환 메커니즘 | PCB, Context Switching, 상태 전이도 |
| **[스레드 모델 및 멀티스레딩](thread)** | 프로세스 vs 스레드 차이, 사용자 수준·커널 수준·혼합형(M:N) 스레딩 모델 비교 | User-level Thread, Kernel-level Thread, M:1, 1:1, M:N |
| **[CPU 스케줄링 알고리즘](cpu-scheduling)** | 비선점형(FCFS·SJF·HRN), 선점형(SRT·Round-Robin·MLFQ), 다단계 피드백 큐 동작 원리 | 기아 현상, 에이징, 타임 퀀텀, MLFQ |
