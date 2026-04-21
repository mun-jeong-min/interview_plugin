# 🎤 interview-me

> 최근 작성한 코드로 기술 면접 연습하기

당신이 작성하거나 수정한 코드를 시니어 엔지니어 관점에서 분석하고, 실제 면접에서 나올 법한 꼬리질문을 생성합니다.

## 설치

```bash
# 1. 마켓플레이스 등록
/plugin marketplace add munjeongmin/interview-me

# 2. 플러그인 설치
/plugin install interview-me@interview-me-marketplace
```

## 사용법

```bash
# 기본: 최근 커밋의 변경사항 분석
/interview-me

# 특정 파일 분석
/interview-me src/auth/TokenService.java

# staged 변경사항만
/interview-me --staged

# 최근 30분 내 수정된 파일
/interview-me --recent=30m

# 질문 개수 조정 (최대 7개)
/interview-me --count=5

# 특정 카테고리만
/interview-me --focus=security,concurrency

# 영어로 질문
/interview-me --lang=en

# 모범 답안 보기
/interview-me --show-answer

# 결과 저장
/interview-me --save
```

## 출력 예시

```
╭──────────────────────────────────────────────────────────────╮
│  🎤  Interview Me  ·  3 questions                            │
│  Target: src/auth/TokenService.java  (last commit, 2m ago)   │
╰──────────────────────────────────────────────────────────────╯

┌─ Q1 ─────────────────────────────── 🔒 Security · ★★☆ ─┐
│                                                        │
│  Refresh token을 평문으로 DB에 저장하고 있는데,        │
│  DB가 유출되면 어떤 공격 시나리오가 가능할까요?        │
│  대응책도 같이 설명해주세요.                           │
│                                                        │
│  📍 TokenService.java:42                               │
│  💡 Hint: hashing, rotation, revocation list, TTL      │
│                                                        │
└────────────────────────────────────────────────────────┘

┌─ Q2 ──────────────────────── ⚡ Concurrency · ★★★ ────┐
│                                                        │
│  saveToken() 메서드가 @Transactional 없이 두 번의      │
│  DB 호출을 하고 있어요. 동시 요청이 들어오면 어떤     │
│  문제가 생길 수 있고, 어떻게 방어할 수 있을까요?      │
│                                                        │
│  📍 TokenService.java:67-81                            │
│  💡 Hint: race condition, 격리 수준, 멱등성           │
│                                                        │
└────────────────────────────────────────────────────────┘

┌─ Q3 ────────────────────────── 🏛  Design · ★★☆ ──────┐
│                                                        │
│  TokenService가 JwtEncoder, UserRepository,            │
│  RedisClient를 직접 주입받고 있네요.                   │
│  Hexagonal architecture 관점에서 뭐가 어색하고,        │
│  어떻게 리팩터링하시겠어요?                            │
│                                                        │
│  📍 TokenService.java:15-22                            │
│  💡 Hint: port/adapter, 의존성 방향, 도메인 순수성     │
│                                                        │
└────────────────────────────────────────────────────────┘

─────────────────────────────────────────────────────────
 💬  답변 준비되면 그냥 이어서 말하면 돼.
     `/interview-me --show-answer` 로 모범 답안 볼 수 있음.
─────────────────────────────────────────────────────────
```

## 질문 카테고리

| 카테고리 | 이모지 | 주요 주제 |
|----------|--------|-----------|
| Security | 🔒 | 인증, 인가, 인젝션, 암호화, 입력 검증 |
| Concurrency | ⚡ | 레이스 컨디션, 트랜잭션, 락, 데드락 |
| Performance | 🚀 | N+1, 캐싱, 인덱싱, 메모리, 지연시간 |
| Design | 🏛 | SOLID, 패턴, 결합도, 추상화, 모듈화 |
| Testing | 🧪 | 커버리지, 모킹, 엣지케이스, 통합테스트 |
| Observability | 👁 | 로깅, 메트릭, 트레이싱, 알림, 디버깅 |
| Data | 🗄 | 스키마, 마이그레이션, 일관성, 무결성 |

## 난이도

- **★☆☆ (기본기)**: 모든 개발자가 알아야 할 기초
- **★★☆ (실무)**: 실제 경험이 필요한 실무 시나리오
- **★★★ (심화)**: 깊은 전문성과 아키텍처 사고 필요

## 사용 시나리오

### 1. 면접 준비

```bash
# 포트폴리오 프로젝트의 핵심 코드 분석
/interview-me src/main/java/com/myapp/core/

# 모범 답안과 함께 연습
/interview-me --show-answer --count=5
```

면접 전에 자신의 코드에 대해 예상 질문을 뽑아보고, 답변을 준비하세요.

### 2. 코드 리뷰 Self-Check

```bash
# PR 올리기 전 staged 코드 점검
/interview-me --staged --focus=security,perf

# 잠재적 문제점 파악
/interview-me --count=7
```

코드 리뷰어가 질문할 만한 포인트를 미리 파악하고 방어 로직을 추가하세요.

### 3. 팀 온보딩

```bash
# 핵심 모듈 분석으로 도메인 이해
/interview-me src/domain/order/ --lang=en

# 결과 저장해서 공유
/interview-me src/core/ --save
```

새 팀원이 코드베이스를 깊이 이해하도록 질문을 생성하고, 함께 논의하세요.

## 옵션 정리

| 옵션 | 설명 | 기본값 |
|------|------|--------|
| `--staged` | staged 변경사항만 분석 | - |
| `--recent=<duration>` | 최근 N분/시간 수정 파일 (예: `30m`, `2h`) | - |
| `--count=<n>` | 질문 개수 | 3 |
| `--focus=<categories>` | 카테고리 필터 (쉼표 구분) | 전체 |
| `--lang=<ko\|en>` | 출력 언어 | ko |
| `--show-answer` | 모범 답안 아웃라인 표시 | 숨김 |
| `--save` | `.interview/` 디렉토리에 저장 | - |

## 질문 품질 기준

**좋은 질문:**
- ✅ 코드의 구체적인 선택에 대한 질문 (왜 이렇게 했나?)
- ✅ 장애 상황 가정 (Redis가 죽으면? DB 커넥션 풀이 차면?)
- ✅ 스케일/엣지케이스 (요청이 10배 오면 어디가 먼저 터지나?)
- ✅ 트레이드오프 (이 구조의 단점은? 언제 안 쓰는 게 좋나?)

**피하는 질문:**
- ❌ "이 함수는 뭘 하나요?" 같은 단순 설명
- ❌ "SOLID가 뭔가요?" 같은 교과서 정의
- ❌ 코드에 없는 내용 상상해서 질문

---

## 플러그인 자체 분석 예시

이 플러그인 코드에 대해 `/interview-me`를 실행한 결과:

```
╭──────────────────────────────────────────────────────────────╮
│  🎤  Interview Me  ·  3 questions                            │
│  Target: interview-me plugin  (self-analysis)                │
╰──────────────────────────────────────────────────────────────╯

┌─ Q1 ───────────────────────────────── 🏛 Design · ★★☆ ─┐
│                                                        │
│  commands/interview-me.md가 코드 수집, 분석, 포맷팅    │
│  로직을 모두 담고 있어요. 이 중 어떤 부분을 별도       │
│  모듈로 분리하면 좋을까요? 분리 기준은 뭘로 잡으시     │
│  겠어요?                                               │
│                                                        │
│  📍 commands/interview-me.md:1-135                     │
│  💡 Hint: SRP, 재사용성, 테스트 용이성, 변경 빈도      │
│                                                        │
└────────────────────────────────────────────────────────┘

┌─ Q2 ────────────────────────── 🧪 Testing · ★★☆ ──────┐
│                                                        │
│  이 플러그인의 질문 생성 품질을 어떻게 테스트하시      │
│  겠어요? LLM 출력의 일관성과 품질을 검증하는 건        │
│  일반 유닛 테스트와 다를 텐데, 어떤 접근을 하시겠어요? │
│                                                        │
│  📍 agents/interviewer.md:24-45                        │
│  💡 Hint: golden set, rubric scoring, human eval, A/B  │
│                                                        │
└────────────────────────────────────────────────────────┘

┌─ Q3 ───────────────────────── 🚀 Performance · ★☆☆ ───┐
│                                                        │
│  --recent=30m 옵션으로 파일을 찾을 때 find 명령을      │
│  사용하고 있어요. 대규모 모노레포(파일 10만개+)에서    │
│  이 방식의 문제점은 뭐고, 어떻게 최적화하시겠어요?     │
│                                                        │
│  📍 commands/interview-me.md:35                        │
│  💡 Hint: git ls-files, fd, inotify, caching           │
│                                                        │
└────────────────────────────────────────────────────────┘

─────────────────────────────────────────────────────────
 💬  답변 준비되면 그냥 이어서 말하면 돼.
     `/interview-me --show-answer` 로 모범 답안 볼 수 있음.
─────────────────────────────────────────────────────────
```

## 라이선스

MIT License
