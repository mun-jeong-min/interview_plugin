# 🎤 interview-me

> 내 프로젝트 코드로 기술 면접 연습하기

프로젝트 전체 코드를 시니어 엔지니어 관점에서 분석하고, 실제 면접에서 나올 법한 꼬리질문을 생성합니다.

## 설치

```bash
# 1. 마켓플레이스 등록
/plugin marketplace add mun-jeong-min/interview_plugin

# 2. 플러그인 설치
/plugin install interview-me@interview-me-marketplace
```

## 사용법

```bash
# 기본: 프로젝트 전체 코드 분석 (질문 3개)
/interview-me

# 질문 개수 조정 (최대 7개)
/interview-me --count=5

# 영어로 질문
/interview-me --lang=en
```

## 옵션

| 옵션 | 설명 | 기본값 |
|------|------|--------|
| `--count=<n>` | 질문 개수 (1~7) | 3 |
| `--lang=<ko\|en>` | 출력 언어 | ko |

## 출력 예시

```
╭──────────────────────────────────────────────────────────────╮
│  🎤  Interview Me  ·  3 questions                            │
│  Target: project codebase                                    │
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
─────────────────────────────────────────────────────────
```
