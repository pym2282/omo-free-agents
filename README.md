# OMO Hybrid Agent Setup v2.0.0

[oh-my-openagent (OMO)](https://github.com/code-yeongyu/oh-my-openagent) 플러그인을 위한 무료 하이브리드 에이전트 설정입니다.

**NVIDIA NIM** (무제한 무료) + **OpenCode Zen** (무료 모델) + **Google AI Studio** (무료 티어) 세 플랫폼을 조합해 유료 구독 없이 최고 수준의 에이전트 팀을 구성합니다.

---

## 🆕 변경 히스토리

### v2.0.0 (2026-04-12)

**주요 변경사항:**

#### ⚠️ 변경된 에이전트 (5개)

| 에이전트 | 이전 모델 | 새 모델 | 변경 이유 |
|---------|----------|--------|----------|
| metis | nvidia/moonshotai/kimi-k2.5 | nvidia/qwen/qwen3-next-80b | 추론 특화 |
| momus | opencode/big-pickle | opencode/minimax-m2.5-free | rate limit 회피 |
| explore | nvidia/meta/llama-4-scout | opencode/gpt-5-nano | NVIDIA 부하 분산 |
| librarian | opencode/big-pickle | opencode/gpt-5-nano | rate limit 회피 |
| multimodal-looker | google/gemini-3-flash | google/gemini-2.5-flash | 안정적 최신 버전 |

#### ➕ 추가된 에이전트 (1개)

- **plan**: opencode/minimax-m2.5-free (계획 수립용)

#### ❌ 제거된 모델

- **big-pickle**: 도구 사용 불안정으로 완전 제외

#### 📊 플랫폼 사용량 변화

| 플랫폼 | v1.x | v2.0 | 변화 |
|--------|------|------|------|
| NVIDIA | 8개 | 6개 | -2 |
| Zen | 4개 | 6개 | +2 |
| Google | 1개 | 1개 | 동일 |

### v1.0.0 (2026-04-10)

- 초기 버전
- NVIDIA NIM 중심 구성

---

## 에이전트 구성

| 에이전트 | 역할 | 모델 | 플랫폼 |
|----------|------|------|--------|
| **sisyphus** | 메인 오케스트레이터 | Kimi K2.5 | NVIDIA |
| **sisyphus-junior** | 경량 오케스트레이터 | GPT-5 Nano | Zen (무료) |
| **hephaestus** | 심층 코딩 | Qwen3 Coder 480B | NVIDIA |
| **atlas** | 실행/빌드 | MiniMax M2.5 Free | Zen (무료) |
| **build** | 빌드 전문 | Devstral 2 123B | NVIDIA |
| **OpenCode-Builder** | 빌드 전문 | Devstral 2 123B | NVIDIA |
| **prometheus** | 계획 추론 | Qwen3-Next 80B Thinking | NVIDIA |
| **metis** | 계획 분석 | Qwen3-Next 80B Thinking | NVIDIA |
| **momus** | 계획 검토 | MiniMax M2.5 Free | Zen (무료) |
| **oracle** | 아키텍처/디버깅 | Qwen3-Next 80B Thinking | NVIDIA |
| **plan** | 태스크 그래프 | MiniMax M2.5 Free | Zen (무료) |
| **explore** | 코드 탐색 | GPT-5 Nano | Zen (무료) |
| **librarian** | 문서 검색 | GPT-5 Nano | Zen (무료) |
| **multimodal-looker** | 이미지/스크린샷 분석 | Gemini 2.5 Flash | Google |

---

## 필요한 것

- [OpenCode](https://opencode.ai) 설치
- [oh-my-openagent](https://github.com/code-yeongyu/oh-my-openagent) 플러그인 설치
- **NVIDIA NIM 계정** (무료)
- **Google AI Studio API 키** (무료)
- OpenCode Zen 구독 (무료 모델만 사용하므로 결제 불필요)

---

## Step 1 — API 키 발급

### NVIDIA NIM API 키

무료 티어: **40 req/min, 일일 상한 없음**

1. [build.nvidia.com](https://build.nvidia.com) 접속
2. 우측 상단 **Sign In** → NVIDIA 계정으로 로그인 (없으면 무료 가입)
3. 우측 상단 프로필 → **API Keys**
4. **Generate API Key** 클릭
5. `nvapi-...` 형태의 키 복사

### Google AI Studio API 키

무료 티어: **15 req/min, 일 1,500 req**

1. [aistudio.google.com](https://aistudio.google.com) 접속
2. Google 계정으로 로그인
3. 좌측 메뉴 **Get API key** 클릭
4. **Create API key** → 프로젝트 선택 후 생성
5. `AIza...` 형태의 키 복사

---

## Step 2 — OpenCode에 API 키 등록

TUI 없이 **CLI 명령어**로 등록합니다. cmd, PowerShell 모두 동작합니다.

```bash
# NVIDIA 키 등록
opencode providers login --provider nvidia

# Google 키 등록
opencode providers login --provider google

# OpenCode Zen 등록 (브라우저 인증)
opencode providers login --provider opencode
```

각 명령 실행 후 API 키 입력 프롬프트가 뜨면 **우클릭**으로 붙여넣기합니다.

등록된 키는 `~/.local/share/opencode/auth.json`에 자동 저장됩니다.

> **Windows 경로:**
> `C:\Users\사용자명\.local\share\opencode\auth.json`

---

## Step 3 — OMO 플러그인 설치

```bash
opencode plugins install oh-my-openagent@latest
```

또는 `~/.config/opencode/opencode.json`에 직접 추가:

```json
{
  "plugin": [
    "oh-my-openagent@latest"
  ]
}
```

---

## Step 4 — 에이전트 설정 적용

`oh-my-openagent.json`을 OpenCode 설정 폴더에 복사합니다.

**Windows (cmd):**
```
copy oh-my-openagent.json %USERPROFILE%\.config\opencode\oh-my-openagent.json
```

**macOS / Linux:**
```
cp oh-my-openagent.json ~/.config/opencode/oh-my-openagent.json
```

OpenCode를 재시작하면 적용됩니다.

---

## 무료 한도 정리

| 플랫폼 | RPM | 일일 상한 | 비고 |
|--------|-----|----------|------|
| NVIDIA NIM | 40 | **없음** | 가장 넉넉함 |
| Google AI Studio | 15 | 1,500 req | multimodal-looker 전용 |
| OpenCode Zen 무료 | - | - | MiniMax M2.5, GPT-5 Nano |

일반적인 코딩 작업에서는 한도에 걸리지 않습니다.

---

## ⚠️ 주의사항

| 모델 | 잠재적 문제 | 대책 |
|------|-------------|------|
| opencode/gpt-5-nano | TUI 선택 불가 | CLI로 직접 지정 |
| opencode/minimax-m2.5-free | quota 초과 가능 | NVIDIA 버전으로 수동 전환 |
| opencode/big-pickle | ❌ 사용 안함 | 도구 사용 불안정 |

---

## Zen 무료 모델 선별 방법

**데이터 소스**: `https://models.dev/api.json`

**선별 기준**:
1. `cost.input === 0` (무료 모델만)
2. `cost` 필드 존재 (로컬 실행형 제외)
3. 제외 프로바이더: github-copilot, gitlab, ollama 등
4. **실제 테스트**: AI 에이전트로 API 호출 및 도구 사용 검증 후 등록

**제외된 모델**:
- `big-pickle`: 도구 사용 불안정
- `glm-4.7-free`, `minimax-m2.1-free`: 2026-03-15 deprecate 예정

---

## Zen 무료 모델 목록 (2026년 4월 기준)

OpenCode Zen에서 무료로 제공되는 모델입니다.

- `opencode/minimax-m2.5-free`
- `opencode/mimo-v2-pro-free`
- `opencode/mimo-v2-omni-free`
- `opencode/nemotron-3-super-free`
- `opencode/gpt-5-nano`

> **참고:** `opencode/big-pickle`은 도구 사용 불안정으로 사용하지 않습니다.

---

## 📋 모델별 특징 및 알려진 문제

| 모델 | 특징 | 알려진 문제 | 권장 사용처 |
|------|------|-------------|-------------|
| opencode/kimi-k2.5-free | 도구 호출 안정적, 멀티모달 지원 | Markdown 표 생성 어려움 | 일반 작업, 도구 사용 |
| opencode/llama-4-maverick | (이전 사용) | "~~하겠습니다"만 하고 실제 미실행 | ❌ 사용 안함 |
| opencode/big-pickle | 스텔스 모델 | 도구 사용 불안정 | ❌ 사용 안함 |

### 상세 설명

**opencode/kimi-k2.5-free**
- ✅ 도구 호출(task)이 실제로 실행됨
- ✅ 멀티모달(이미지/비디오/PDF) 지원
- ⚠️ 표(table) 생성 시 Markdown 형식 오류

**opencode/llama-4-maverick (사용 중단)**
- ❌ "하겠습니다" 등 의지 표현만 하고 실제 작업 미완료
- ❌ 도구 호출 후 결과 미반환
- → Kimi로 복귀

**opencode/big-pickle (사용 중단)**
- ❌ 도구 사용 불안정
- ❌ 전반적인 신뢰성 이슈
