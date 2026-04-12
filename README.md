# OMO Hybrid Agent Setup

[oh-my-openagent (OMO)](https://github.com/code-yeongyu/oh-my-openagent) 플러그인을 위한 무료 하이브리드 에이전트 설정입니다.

**NVIDIA NIM** (무제한 무료) + **OpenCode Zen** (무료 모델) + **Google AI Studio** (무료 티어) 세 플랫폼을 조합해 유료 구독 없이 최고 수준의 에이전트 팀을 구성합니다.

---

## 에이전트 구성

| 에이전트 | 역할 | 모델 | 플랫폼 |
|----------|------|------|--------|
| **sisyphus** | 메인 오케스트레이터 | Kimi K2.5 | NVIDIA |
| **sisyphus-junior** | 경량 오케스트레이터 | GPT-5 Nano | Zen (무료) |
| **hephaestus** | 심층 코딩 | Qwen3 Coder 480B | NVIDIA |
| **atlas** | 실행/빌드 | MiniMax M2.5 | Zen (무료) |
| **build** | 빌드 전문 | Devstral 2 123B | NVIDIA |
| **OpenCode-Builder** | 빌드 전문 | Devstral 2 123B | NVIDIA |
| **prometheus** | 계획 추론 | Qwen3-Next 80B Thinking | NVIDIA |
| **metis** | 계획 분석 | Kimi K2.5 | NVIDIA |
| **momus** | 계획 검토 | Big Pickle | Zen (무료) |
| **oracle** | 아키텍처/디버깅 | Qwen3-Next 80B Thinking | NVIDIA |
| **plan** | 태스크 그래프 | Kimi K2.5 | NVIDIA |
| **explore** | 코드 탐색 (10M ctx) | Llama 4 Scout | NVIDIA |
| **librarian** | 문서 검색 | Big Pickle | Zen (무료) |
| **multimodal-looker** | 이미지/스크린샷 분석 | Gemini 3 Flash | Google |

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
| OpenCode Zen 무료 | - | - | Big Pickle, MiniMax M2.5, GPT-5 Nano |

일반적인 코딩 작업에서는 한도에 걸리지 않습니다.

---

## Zen 무료 모델 목록 (2026년 4월 기준)

OpenCode Zen에서 무료로 제공되는 모델입니다. 무료 기간이 종료될 수 있습니다.

- `opencode/big-pickle`
- `opencode/minimax-m2.5-free`
- `opencode/mimo-v2-pro-free`
- `opencode/mimo-v2-omni-free`
- `opencode/qwen3.6-plus-free` *(현재 일부 환경에서 오류 발생)*
- `opencode/nemotron-3-super-free`
- `opencode/gpt-5-nano`
