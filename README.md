# 안녕하세요, 노현우입니다 👋

> **"왜?"라는 질문에서 멈추지 않는 백엔드 개발자**

작곡 전공 수석에서 IT 전공 수석으로 — 새로운 도메인에서도 본질을 파고드는 학습력으로 일합니다.
부트로더부터 파일 시스템까지 OS를 직접 구현해 본 기초 체력을 바탕으로, 프레임워크 내부 동작을 이해하고 최적의 성능을 이끌어내는 개발자가 되고자 합니다.

---

## 🧑‍💻 About

- 🎓 **성공회대학교 졸업** · 정보통신공학(주) / 컴퓨터공학(부) / 소프트웨어공학(부)
- 🏆 **수석 졸업** · GPA 4.49 / 4.5
- 📜 AWS Certified Solutions Architect – Associate · 정보처리기사

## 🛠️ Tech Stack

`Java` `Spring Boot(MVC)` `Spring Boot(WebFlux)` `JPA` `QueryDSL`
`Python` `FastAPI` `C` `Ruby` `Assembly`
`MySQL` `PostgreSQL (pgvector)` `Redis` `Docker` `AWS`

---

## 🚀 Projects

### 🎮 [웹 1:1 고스톱 게임 서버](https://github.com/poming-matgo/poming_matgo_be)
백엔드(https://github.com/poming-matgo/poming_matgo_be)
프론트(https://github.com/poming-matgo/poming_matgo_fe)
실시간 양방향 통신이 필요한 1:1 카드 게임 서버

- Redis 의존성을 제거하고 `ConcurrentHashMap` 기반 로컬 구조로 전환 → **초당 약 68,000건의 웹소켓 메시지 무손실 처리 (Redis 구성 대비 6.6배)**
- WebFlux 논블로킹 철학을 유지하는 동시성 제어 — In-Flight 플래그 기반 race 해결, 방 단위 / 턴 단위 락 정책 분리
- `System.nanoTime()` 기반 Monotonic Clock + 서버 유예 시간으로 타임아웃 신뢰성 확보

`Java` `Spring WebFlux` `WebSocket` `Redis` `React`

### 💻 [SKHU OS — 운영체제 직접 구현](https://github.com/nhwgit/skhuos)
하드웨어 부팅부터 애플리케이션 실행까지 가능한 독자 운영체제

- BIOS 부트로더, 16/32/64비트 모드 전환, GDT/IDT, 인터럽트 핸들러 직접 구현
- PIT 타이머 기반 라운드 로빈 스케줄러 + 어셈블리 레벨 컨텍스트 스위칭
- 스핀락 무한 루프 대기 문제를 원자적 연산 + CPU yield로 개선
- PIO 모드 디스크 드라이버, FAT 구조 자체 파일 시스템, 시스템 콜

`C` `Assembly`

### 🏆 [백준(BOJ) 숏코딩 전체 1위](https://github.com/nhwgit/shortCoding)
[BOJ 1006](https://www.acmicpc.net/short/status/1006) · [10350번](https://www.acmicpc.net/short/status/10350) **숏코딩 전체 1위**

- Ruby 인터프리터의 바이트코드 동작 분석 → 시간 초과 회피와 코드 길이 최소화의 트레이드오프 지점 도출
- 루프 내 메서드 호출 오버헤드를 포착해 `i=-~i%n` → `(i+=1)%n` 으로 리팩토링, 글자 수 × 실행 속도 최적점 달성
- 채점 서버 런타임 변경(YJIT 추정) 추적 → 이전엔 시간 초과였던 패턴을 재도입해 **1위 탈환**

`Ruby`

### 📐 [Algebraic OOP Graph Library](https://github.com/nhwgit/Algebraic-OOP-Graph)
"추상대수와 객체지향은 본질적으로 같다"는 철학으로 설계한 그래프 알고리즘 라이브러리

- "Semiring 원소를 갖는 N×N 행렬은 그 자체로 Semiring" 정리를 `MatrixSemiring<T> implements Semiring<Matrix<T>>` 로 구현해 재귀적 추상화 달성
- 동일한 행렬 거듭제곱 코드가 Semiring 구현체 교체만으로 **경로의 수 / 최단경로(Min-Plus) / 도달 가능성** 3가지 문제를 해결 (전략 패턴)
- Monoid 결합법칙을 근거로 비트 연산 + 분할 정복 거듭제곱을 default method로 구현해 **O(log N)** 확보

`Java`

### 🔍 [RAG Evaluation Harness on pgvector](https://github.com/nhwgit/RAG-Eval-Harness-on-pgvector)
"검색이 잘 된다"가 아니라 "얼마나, 왜 잘 되는지"를 측정하는 RAG 평가 파이프라인

- pgvector + HNSW 채택, 청킹/top-k/query 변형을 동일 평가셋에서 **recall@k · MRR · LLM-as-judge**로 정량 비교
- 60문항 한국어 평가셋을 `type(7) × difficulty(3)` 두 축으로 라벨링 → 카테고리별 약점 진단 가능
- `recall@5 ≈ 0.83` 정체 현상을 알고리즘이 아닌 **평가셋 라벨링 오류**로 진단, 데이터 품질 검증을 파이프라인에 통합
- retrieval 천장 효과를 발견하고 hybrid retrieval 실험을 보류 — 측정 가능한 차이가 보장되지 않는 실험을 거르는 판단

`Python` `FastAPI` `PostgreSQL (pgvector/HNSW)` `OpenAI API` `Docker`

### 📝 교육 기관 퀴즈 시스템 *(외주 프리랜서 계약)*
강사·학생용 퀴즈 출제/채점 백엔드 시스템 구축 및 납품

- 문항 수정 후에도 과거 응답·통계가 변형되지 않도록 **스냅샷 아키텍처** 도입
- 통계 쿼리 병목을 예측해 반정규화 테이블 선제 구축 → **100만 건 환경에서 통계 조회 평균 0.2초 이내**
- Parallel Stream 병렬 암호화 + JDBC Batch Insert 로 1만 건 엑셀 업로드 성능 **20배 향상**
- `@Version` 낙관적 락 + 재시도 로직으로 공유 계정 Lost Update 방지

`Java` `Spring Boot` `MySQL` `JPA` `QueryDSL` `Docker`

---

## 💬 Experience

- **교내 IT 커뮤니티 운영** — 성공회대학교 (2020.03 ~ 2022.02)
  350명 규모 전공 정보 공유 커뮤니티 운영 / 학부생 설문 분석 기반 전공 커리큘럼 개편 주도
- **컴퓨터공학과 TA** — 성공회대학교 (2022.09 ~ 2022.12)
  학부생 소켓 프로그래밍 과제 평가 및 멘토링
