# AI Terminal Hub

Claude Code · OpenAI Codex · Gemini CLI · Ollama(Gemma)를 한 창에서 통합 실행하는 Electron 앱.

- 멀티 터미널 · 그리드 정렬 · 분리·재결합
- 메인 규칙 자동 주입 (Karpathy 4원칙 + 응답 원칙 + 토큰 절약 가이드)
- Claude Code 전역 Subagent (`~/.claude/agents/`) 관리 — 분야별 폴더 + AI 추천 생성
- 터미널 내 검색 (Ctrl+F), 표준 단축키, WebGL 가속

## 다운로드

[Releases](../../releases) 페이지에서 최신 `AI-Terminal-Hub-x.y.z-portable.exe` 다운로드.

설치 없이 더블클릭으로 실행됩니다.

## 시스템 요구사항

| 항목 | 요구 |
|---|---|
| OS | **Windows 10/11 (x64)** |
| RAM | 4 GB 이상 (8 GB 권장) |
| 디스크 | 200 MB |
| Node.js | **필수 — 18 이상** (CLI 도구 설치에 npm 사용) |
| Git | 권장 (워크스페이스 정보 표시용) |

이 앱은 자체로 AI를 호출하지 않고 **사용자 PC의 CLI 도구**를 PTY로 실행합니다. 따라서 사용하려는 AI마다 별도 설치 필요:

```powershell
npm install -g @anthropic-ai/claude-code   # Claude Code
npm install -g @openai/codex               # OpenAI Codex
npm install -g @google/gemini-cli          # Gemini CLI
# Ollama는 https://ollama.com 에서 설치 후 `ollama pull gemma3`
```

앱 첫 실행 시 자동으로 설치 상태를 확인하고 미설치 도구는 모달에서 한 번에 설치할 수 있습니다 (이후엔 도구바의 ⚙ 버튼으로 재진입).

## 첫 실행 — 보안 경고 회피

이 앱은 **코드 서명 인증서가 없습니다** (인증서는 유료 — DigiCert 등 $100+/년). 그래서 처음 실행 시 Windows가 경고를 띄웁니다:

> **Windows에서 PC를 보호했습니다**
> Microsoft Defender SmartScreen이 알 수 없는 앱을 실행하지 못하게 했습니다…

**회피 방법**:
1. 경고 창의 **"추가 정보"** 클릭
2. 하단의 **"실행"** 버튼 클릭

한 번 허용하면 다음부터는 안 뜹니다. 무결성을 검증하고 싶다면 Release 페이지의 SHA256 체크섬과 비교:

```powershell
Get-FileHash .\AI-Terminal-Hub-0.1.0-portable.exe -Algorithm SHA256
```

## 백신 오탐 (false positive)

Electron으로 패키징된 unsigned exe는 일부 백신이 의심 파일로 분류할 수 있습니다 (실제 악성코드 X). 다음 중 하나로 해결:
- 백신의 **예외 목록**에 추가
- **VirusTotal**에 업로드해 결과 확인 (다수 엔진이 clean이면 오탐)

## 사용자 데이터 위치

- 워크스페이스·사용자 규칙: `%APPDATA%\AI Terminal Hub\`
- 에이전트 정의: `%USERPROFILE%\.claude\agents\` (Claude Code 표준 위치)

앱을 삭제해도 데이터는 보존됩니다. 완전 제거는 위 폴더 직접 삭제.

## 단축키

| 키 | 동작 |
|---|---|
| `Ctrl+T` | 새 PowerShell 세션 |
| `Ctrl+W` | 활성 세션 닫기 |
| `Ctrl+Tab` / `Ctrl+Shift+Tab` | 다음·이전 세션 |
| `Ctrl+1`~`9` | 번호로 세션 활성화 |
| `Ctrl+Shift+G` | 그리드 정렬 토글 |
| `Ctrl+F` | 터미널 출력 검색 |
| 탭 더블클릭 | 세션 별칭 편집 |

## 라이선스

**Copyright (c) 2025 plz_. All Rights Reserved.**

개인적 평가·학습·비상업적 사용만 허용. 무단 수정·재배포·상업적 이용·판매 금지.
자세한 조건은 `LICENSE` 파일 참조.

이 라이선스 범위를 넘어서는 사용은 저작권자의 사전 서면 허락이 필요합니다.
