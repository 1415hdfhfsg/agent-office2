# AI Terminal Hub

Claude Code · OpenAI Codex · Gemini CLI · Ollama(Gemma)를 한 창에서 통합 실행하는 Electron 앱.

- 멀티 터미널 · 그리드 정렬 · 분리·재결합
- 메인 규칙 자동 주입 (Karpathy 4원칙 + 응답 원칙 + 토큰 절약 가이드)
- Claude Code 전역 Subagent (`~/.claude/agents/`) 관리 — 분야별 폴더 + AI 추천 생성
- 터미널 내 검색 (Ctrl+F), 표준 단축키, WebGL 가속
- HTML·이미지 화면을 드래그해 구역별 수정 지시를 남기는 Visual Review
- 승인 기반 Windows Use (창 캡처·UI Automation·클릭·입력·긴급 중단)
- Codex·Claude 공용 로컬 MCP 도구 연결
- 사용자 요청·진행상태·질문·답변·완료 결과를 주고받는 AI 작업함
- 저해상도 우선 캡처·UIA 검색/개수 제한·압축 응답으로 MCP 토큰 사용 절감
- 현재 선택한 프로젝트의 요구·설계·코드·기능·테스트·보안·성능·문서를 평가하는 100점 종합평가
- 모든 Claude·Codex·Gemini·Gemma 모델에 Ponytail 최소 구현 규칙 자동 적용

## 다운로드

[Releases](../../releases) 페이지에서 최신 `AI-Terminal-Hub-Setup-x.y.z.exe` 다운로드.

설치 파일을 실행한 뒤 시작 메뉴 또는 바탕화면 바로가기로 실행합니다.

현재 검증 배포: **v0.1.22**

- 앱 안에서 최신 버전을 내려받고 종료 후 자동 설치·재실행
- 자동 업데이트 실패 시 기존 최신 설치기 직접 다운로드 기능 유지
- 실행 중인 터미널 수를 안내한 뒤 사용자 승인으로 설치
- GitHub 릴리스의 `latest.yml`·blockmap으로 버전·무결성·차등 다운로드 지원
- 앱 제작자 메타데이터를 GitHub 이름 `1415hdfhfsg`로 통일

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
Get-FileHash .\AI-Terminal-Hub-Setup-x.y.z.exe -Algorithm SHA256
```

## 백신 오탐 (false positive)

Electron으로 패키징된 unsigned exe는 일부 백신이 의심 파일로 분류할 수 있습니다 (실제 악성코드 X). 다음 중 하나로 해결:
- 백신의 **예외 목록**에 추가
- **VirusTotal**에 업로드해 결과 확인 (다수 엔진이 clean이면 오탐)

## 사용자 데이터 위치

- 워크스페이스·사용자 규칙·코드/시각 주석·AI 작업함·품질 평가 기록: `%APPDATA%\ai-terminal-hub\`
- 에이전트 정의: `%USERPROFILE%\.claude\agents\` (Claude Code 표준 위치)

앱을 삭제해도 데이터는 보존됩니다. 완전 제거는 위 폴더 직접 삭제.

## Windows Use 안전 범위

- 일반 사용자 권한의 캡처 가능한 창만 대상으로 합니다.
- 클릭·문자 입력·키 입력·UI 요소 실행은 Terminal Hub 승인창을 거칩니다.
- 실행 중에는 Windows Use 창의 **긴급 중단** 또는 `Esc`로 중단할 수 있습니다.
- UAC·Windows 보안·자격 증명·결제·금융·Codex/ChatGPT·터미널 창은 대상에서 제외합니다.
- Windows Use 창의 **AI 연결 설정**을 한 번 실행한 뒤 새 AI 세션에서 도구를 사용할 수 있습니다.

두 도구는 `파일/창 선택 → 영역/대상 지정 → 작업 실행 → 결과 확인` 순서를 화면 상단에 표시합니다. Windows Use에서는 창 이름 검색과 화면 재캡처를 사용할 수 있고, Visual Review에서는 실행 중인 Codex·Claude 세션을 화면에서 바로 확인할 수 있습니다.

Visual Review에서 영역별 지시를 저장한 뒤 오른쪽의 **AI에게 수정 맡기기**에서 다음 중 하나를 선택할 수 있습니다.

- 실행 중인 Codex·Claude 터미널을 정확히 선택해 미완료 지시 전체 전달
- `전달 후 Enter 자동 입력`을 켜 사용자가 명시적으로 바로 실행
- AI 세션이 없어도 작업함에 등록하고, 나중에 진행상태·질문·결과 확인

## AI 작업함과 토큰 절약

- **AI 작업함**에 요청을 저장하면 연결된 Codex·Claude가 `task_inbox`로 가져가 진행상태와 결과를 기록할 수 있습니다.
- 에이전트 질문은 작업함 배지와 알림으로 표시되며, 사용자 답변은 저장 후 해당 터미널 입력창에 확인 문구로 전달됩니다. 안전을 위해 Enter는 자동 입력하지 않습니다.
- Windows/Visual Review 캡처는 MCP에서 저해상도 JPEG를 기본 사용하고, 글자가 읽히지 않을 때만 고해상도를 요청합니다.
- UI Automation은 기본 60개 요소만 반환하며 `query` 검색으로 필요한 이름·자동화 ID·컨트롤 종류만 좁힐 수 있습니다.

## 단축키

| 키 | 동작 |
|---|---|
| `Ctrl+T` | 새 PowerShell 세션 |
| `Ctrl+W` | 활성 세션 닫기 |
| `Ctrl+Tab` / `Ctrl+Shift+Tab` | 다음·이전 세션 |
| `Ctrl+1`~`9` | 번호로 세션 활성화 |
| `Ctrl+Shift+G` | 그리드 정렬 토글 |
| `Ctrl+Shift+A` | HTML·이미지 영역 주석 열기 |
| `Ctrl+Shift+U` | Windows 화면 제어 열기 |
| `Ctrl+Shift+I` | AI 작업함 열기 |
| `Ctrl+Shift+B` | 현재 프로젝트 종합평가 열기 |
| `Ctrl+F` | 터미널 출력 검색 |
| 탭 더블클릭 | 세션 별칭 편집 |

## 프로젝트 종합평가

상단의 **종합평가**에서 현재 선택한 작업 폴더의 구조·테스트·보안·문서를 자동 점검하고, 실제 기능·예외 처리·성능·사용성은 실행 근거와 함께 평가할 수 있습니다. 기록은 프로젝트별로 분리되며 미실행 항목은 0점과 평가 커버리지로 구분됩니다. 핵심 품질 항목이 실패하면 종합 점수는 최대 59점으로 제한되고 이전 평가 대비 변화와 Markdown 보고서를 확인할 수 있습니다.

## Ponytail 공통 규칙

[DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail)의 최소 구현 원칙을 Claude·Codex·Gemini·Gemma의 모든 모델에 자동 적용합니다. 새 기능을 만들기 전에 현재 필요성, 기존 코드, 표준 라이브러리, 플랫폼 기본 기능과 설치된 의존성을 차례로 확인하며 보안·입력 검증·접근성·명시 요구와 검증은 단순화하지 않습니다. Claude·Codex·Gemini의 새 세션과 이어하기 세션에는 시스템·개발자 규칙으로, Gemma에는 세션 시작 지침으로 적용됩니다.

## 라이선스

**Copyright (c) 2025-2026 1415hdfhfsg. All Rights Reserved.**

개인적 평가·학습·비상업적 사용만 허용. 무단 수정·재배포·상업적 이용·판매 금지.
자세한 조건은 `LICENSE` 파일 참조.

이 라이선스 범위를 넘어서는 사용은 저작권자의 사전 서면 허락이 필요합니다.
