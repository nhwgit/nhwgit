# 안녕하세요, 노현우입니다 👋

> **"근본까지 파고들어 직접 확인한다" — 시스템을 이해하고 만드는 백엔드 개발자**

작곡 전공 수석(선화예중)에서 IT 전공 수석 졸업(성공회대, 4.49/4.5)으로 — 도메인이 바뀌어도 본질을 파고드는 방식은 변하지 않았습니다.
부트로더부터 파일시스템까지 OS를 직접 구현하며 동시성과 I/O를 **시스템 아키텍처 관점**에서 다루는 시각을 길렀고, 같은 시각으로 Java 비동기 서버의 race condition을 계층별 방어로 풀어 **초당 68,000건을 무손실 처리**했습니다.

---

## 🧑‍💻 About

- 🎓 **성공회대학교 졸업** · 정보통신공학(주) / 컴퓨터공학(부) / 소프트웨어공학(부)
- 🏆 **수석 졸업** · GPA 4.49 / 4.5
- 🪖 병역필 (2023.07 ~ 2025.04)
- 📜 AWS Certified Solutions Architect – Associate · 정보처리기사

## 🛠️ Tech Stack

**Language** &nbsp;`Java` `Python` `C` `Ruby` `Assembly`
**Backend** &nbsp;`Spring Boot (MVC)` `Spring Boot (WebFlux)` `JPA` `QueryDSL` `FastAPI`
**Data** &nbsp;`MySQL` `PostgreSQL (pgvector)` `Redis`
**Infra** &nbsp;`Docker` `AWS` `Spring Cloud Gateway` `Keycloak`

---

## 🚀 주요 프로젝트

### 🎮 비동기 실시간 고스톱 게임 서버
[백엔드](https://github.com/poming-matgo/poming_matgo_be) · [프론트](https://github.com/poming-matgo/PomingMatgo_fe)

WebFlux 논블로킹 환경에서 실시간 양방향 통신을 처리하는 1:1 카드 게임 서버

- **계층적 동시성 설계** — `k6`로 동시접속 10,000 부하를 걸어 race condition 발생 지점을 찾고, 도메인 로직 → In-Flight 플래그(유저 > 자동플레이) → AOP 턴 직렬화 → 스케줄러(시퀀스 기반 Task 교체)로 책임을 분리해 방어
- **I/O 병목 제거 (처리량 6.6배)** — 1:1 세션 상태가 휘발성·단일 인스턴스 귀속이라는 도메인 특성에 착안, Redis를 `ConcurrentHashMap`으로 대체해 **초당 약 68,000건 무손실 처리** (Profile 분리로 Redis 롤백도 가능)
- **타이머 신뢰성** — 서버 측 유예 시간 + `Monotonic Clock`으로 NTP 동기화 오류·시스템 시간 변경에 영향받지 않는 타임아웃 구현

`Java` `Spring WebFlux` `WebSocket` `Redis` `k6` `React`

### 💻 [SKHU OS — 운영체제 직접 구현](https://github.com/nhwgit/skhuos)
하드웨어 부팅부터 애플리케이션 실행까지 가능한 독자 OS *(학부 캡스톤 · 2022.03 ~ 06)*

- BIOS 부트로더, 16/32/64비트 모드 전환, GDT/IDT, 인터럽트 핸들러 직접 구현
- PIT 타이머 기반 라운드 로빈 스케줄러 + 어셈블리 레벨 컨텍스트 스위칭
- 스핀락 무한 루프 대기로 인한 CPU 낭비를 원자적 연산 + 즉각적 CPU 양보로 개선
- PIO 모드 디스크 드라이버, FAT 구조 자체 파일 시스템, 시스템 콜

`C` `Assembly`

## 🔐 [제로트러스트 접근제어 게이트웨이](https://github.com/nhwgit/zero_trust)

NIST SP 800-207 기준 PEP/PDP/PIP를 직접 구현. Keycloak JWT 검증 후 데이터플레인
게이트웨이(PEP)가 PDP 정책 결정을 시행하고, 위험 판단을 커널 레벨 트래픽 제어(XDP)까지 연결

- **지속 검증 / 위험 적응 인가** — PIP가 위험 점수(IP 변화·요청 레이트 등)를 산출, 위험 상승 시
  능동 캐시 무효화로 재로그인 없이 `ALLOW → DENY` 전이 (다중 게이트웨이는 Redis pub/sub)
- **커널 레벨 트래픽 제어 (eBPF/XDP)** — XDP가 per-source-IP SYN 카운트를 PIP 위험 신호로
  올리고(관측), PIP의 차단 지시(deny + TTL)를 커널 deny map에 반영해 위험 IP 패킷을 스택 진입
  전 드랍(집행). 판단은 PIP, 집행은 커널 — 동일 SYN 플러드에서 게이트웨이 CPU **108% → 0.2%**
- **패킷 레벨 관측** — netns + `tcpdump`로 서비스 간 mTLS를 캡처해 상호 인증을 와이어로 입증,
  `tc netem`으로 L4 장애(지연/손실)를 주입해 재전송·p99 진단
- **운영/테스트** — RED 지표 + Grafana, 요청 ID 분산 추적, PDP 장애 fail-close 재현, k6 부하
  검증, Testcontainers(Keycloak) e2e

`Java` `Spring Cloud Gateway` `Keycloak` `eBPF/XDP` `Go` `mTLS` `Prometheus/Grafana` `Redis` `Docker`

### 📝 교육 SaaS 백엔드 단독 개발 *(프리랜서)*
강사·학생용 퀴즈 출제/채점 백엔드를 단독 설계·구현·납품 *(A사 발주 / B사 수주)*

- **조회 전용 반정규화 모델** — 통계 조인 깊이가 계속 늘어나는 구조적 한계를 판단, 학습 기록이 풀이 시점에 확정되는 불변 데이터라는 특성에 착안해 풀이 결과를 그대로 적재 → 반정규화의 주된 비용인 정합성 문제 자체를 제거
- **개인정보 컴플라이언스 단독 설계** — 법정대리인 연락처 기반 지점 매개 가입(B2B2C), 지점 단위 멀티테넌트 접근 제어, 민감·고유식별정보 미수집을 처리방침과 데이터 모델·권한 구조 차원에서 일치
- **하네스 엔지니어링** — AI 에이전트에 명세·검증·권한 경계를 부여하는 하네스를 설계하고, 사람이 정의한 검증 루프로 산출물 품질을 통제하며 전 모듈을 단기간에 구현

`Java` `Spring Boot` `MySQL` `JPA` `QueryDSL` `Docker` `Claude Code`
---

## 🧩 Special — 호기심에서 출발한 프로젝트

### 📐 [Algebraic OOP Graph Library](https://github.com/nhwgit/Algebraic-OOP-Graph)
"추상대수와 객체지향은 본질적으로 같다"는 철학으로 설계한 그래프 알고리즘 라이브러리

- "Semiring 원소를 갖는 N×N 행렬은 그 자체로 Semiring" 정리를 `MatrixSemiring<T> implements Semiring<Matrix<T>>`로 구현해 재귀적 추상화 달성
- 동일한 행렬 거듭제곱 코드가 구현체 교체만으로 **경로의 수 / 최단경로(Min-Plus) / 도달 가능성** 3문제를 해결 (전략 패턴)
- Monoid 결합법칙을 근거로 비트 연산 + 분할 정복 거듭제곱을 default method로 구현해 **O(log N)** 확보

`Java`

### 🏆 [백준(BOJ) 숏코딩 전체 1위](https://github.com/nhwgit/shortCoding)
백준 온라인 저지의 대표 문제군에 속하는 **1006번·10350번 숏코딩 1위**

- Ruby 인터프리터의 바이트코드 동작을 분석해 시간 초과 회피와 코드 길이 최소화의 트레이드오프 지점 도출
- 루프 내 메서드 호출 오버헤드를 포착해 `i=-~i%n` → `(i+=1)%n`으로 리팩토링
- 채점 서버 런타임 변경(YJIT 추정)을 실행 시간 데이터로 추론해 이전 패턴을 재도입, **1위 탈환**

`Ruby`

---

## 💬 Experience & Community

- **교내 IT 커뮤니티 개설·운영** — 성공회대학교 (2020.03 ~ 2022.02)
  코로나19 비대면 환경의 소통 단절을 해결하기 위해 350명 규모 전공 정보 공유 커뮤니티 운영. 학부생 설문 분석을 바탕으로 교수진 면담을 진행해 전공 커리큘럼 개편·통합을 이끌어냄
- **네트워크 트랙 TA** — 성공회대학교 (2022.09 ~ 2022.12)
  소켓 프로그래밍(TCP 예외 처리·직렬화·메모리 정렬·리소스 누수)과 Wireshark 패킷 분석 멘토링
- **학사정보시스템 IDOR 취약점 발견·신고**
  권한 검증 누락 취약점(IDOR)을 발견, 정보주체 동의 하에 검증 후 학내 정보 보안 책임자에게 책임 공개(responsible disclosure) 절차를 거쳐 보고

---

## 📜 Certificate

- **AWS Certified Solutions Architect – Associate** (2026.02 / AWS)
- **정보처리기사** (2023.10 / 한국데이터산업진흥원)
