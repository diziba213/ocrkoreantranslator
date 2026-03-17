# HBS Case Bilingual Viewer

Harvard Business School 케이스 스터디를 영한 대조로 읽을 수 있는 정적 웹 뷰어.
단어를 더블클릭하면 VC/경영 맥락에 맞는 한국어 설명을 제공한다.

## Why

KAIST MBA(BIZ.60010 Venture Capital, 백용욱 교수) 수업의 케이스 스터디는 영어 원문으로 제공된다. 비영어권 학생에게 영문 케이스를 읽으면서 동시에 한국어 번역과 용어 설명을 볼 수 있는 도구가 필요했다.

## Architecture

```
[사용자] ── 더블클릭 ──> [뷰어 HTML]
                          │
                    ┌─────┴─────┐
                    │ terms.json │  ← 사전 생성된 용어 설명
                    └───────────┘

뷰어 = 단일 HTML 파일 (순수 HTML/CSS/JS, 프레임워크 없음)
호스팅 = GitHub Pages (정적, 무료)
```

### 로컬 개발 모드 (선택적)

로컬에서는 Ollama + qwen3.5:2b를 연결해 실시간 용어 설명을 생성할 수 있다.

```
[사용자] ── 더블클릭 ──> [뷰어 HTML] ──> [server.py] ──> [Ollama API]
                                                          (qwen3.5:2b)
```

## Quick Start

### 온라인 (GitHub Pages)
```
# 배포 후 URL로 접속 — 비밀번호 필요
```

### 로컬 개발 (server.py 방식)
```bash
# 1. Ollama 설치 및 모델 다운로드
ollama pull qwen3.5:2b

# 2. 서버 실행
python server.py

# 3. 브라우저 자동 오픈: http://localhost:8080/Athleta_bilingual_v3.html
```

### 로컬 LLM 모드 (GitHub Pages에서 Ollama 직접 연결)

GitHub Pages에서 로컬 Ollama를 사용하려면 CORS 허용이 필요하다.

```bash
# Windows (PowerShell)
$env:OLLAMA_ORIGINS="*"; ollama serve

# macOS / Linux
OLLAMA_ORIGINS=* ollama serve
```

설정 후 페이지를 새로고침하면 배지가 `AI: qwen3.5:2b` (초록)로 바뀌며 로컬 LLM 모드로 전환된다.

> **참고:** GitHub Pages(HTTPS) → Ollama(HTTP localhost) 연결은 브라우저의 Mixed Content 정책으로 차단될 수 있다.
> 이 경우 HTML 파일을 직접 다운로드하여 `file://` 로 열거나 `python server.py`를 사용한다.

## Project Structure

```
22. Case02_Athleta/
├── README.md                       ← 이 파일
├── CLAUDE.md                       ← Claude Code 지시문
├── server.py                       ← 로컬 Ollama 프록시 서버
├── Athleta_bilingual_v3.html       ← 메인 뷰어 (영한 대조 + AI 설명)
├── terms.json                      ← VC/경영 용어 사전 (~80개)
├── Athleta_bilingual_v2.html       ← v2 (아카이브)
├── Athleta_bilingual_sample.html   ← 초기 프로토타입 (아카이브)
├── Athleta_KR.md                   ← 한국어 전문 번역 (markdown)
├── 2. Athleta-Discussion Q.pdf     ← 수업 토론 질문지 (원본)
├── Captures/                       ← HBS 케이스 스크린샷 22장
│   └── Screenshot 2026-03-17 *.png
├── devlog/
│   └── 2026-03-18.md               ← 개발 일지
└── .claude/                        ← Claude Code 세션 데이터
```

## Tech Stack

| 레이어 | 기술 | 선택 이유 |
|---|---|---|
| 뷰어 | Vanilla HTML/CSS/JS | 프레임워크 없이 단일 파일 배포. GitHub Pages 호환 |
| 로컬 LLM | Ollama + qwen3.5:2b | 무료, 로컬 실행, thinking 모드 off로 빠른 응답 |
| 프록시 | Python stdlib (`http.server`) | 의존성 0, 표준 라이브러리만 사용 |
| 번역 | Claude Code Vision | 스크린샷 OCR + 한국어 번역을 단일 세션에서 처리 |
| 호스팅 | GitHub Pages | 무료 정적 호스팅 |

## Copyright

본 프로젝트의 케이스 원문 저작권은 Harvard Business School Publishing에 있습니다.
교육 목적(Fair Use)으로만 사용되며, 상업적 이용 및 재배포를 금합니다.
저작권자 요청 시 즉시 삭제합니다.

## Author

KAIST IMMS — BIZ.60010 Venture Capital (2026 Spring)
