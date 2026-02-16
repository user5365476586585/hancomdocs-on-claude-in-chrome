# 한컴독스 웹 자동화 에이전트

한컴독스(Hancom Docs) 웹 에디터에서 HWP(한글) 문서를 자동으로 생성/편집/관리하는 Claude Code 기반 에이전트 시스템.

## 개요

- **목적**: 브라우저 자동화(CDP)를 통해 한컴독스 웹 에디터의 문서 작업을 자동화
- **범위**: HWP(한글) 문서 전용. 한셀/한쇼/한워드 등 다른 문서 유형은 미지원
- **방식**: Claude Code CLI + Claude in Chrome 확장을 통한 브라우저 제어
- **언어**: 한국어 환경에서 개발/테스트됨. 에디터 UI, skill 프롬프트, 문서 작성 모두 한국어 기준

## 필수 환경

| 구성 요소 | 요구 사항 |
|-----------|----------|
| Chrome 브라우저 | 최신 버전 |
| Claude Code CLI | [설치 가이드](https://docs.anthropic.com/en/docs/claude-code) |
| Claude in Chrome | Chrome 웹 스토어에서 확장 설치 |
| 한컴독스 계정 | [hancomdocs.com](https://www.hancomdocs.com) 가입 필요 |

## 설치

```bash
# 1. 저장소 클론
git clone https://github.com/user5365476586585/hancomdocs-on-claude-in-chrome.git
cd hancomdocs-on-claude-in-chrome

# 2. Claude Code CLI 설치 (미설치 시)
# https://docs.anthropic.com/en/docs/claude-code 참조
```

### Claude in Chrome 확장 설정

1. Chrome 웹 스토어에서 **Claude in Chrome** 검색 → 설치
2. 확장 아이콘 클릭 → Anthropic API 키 또는 Claude 계정으로 로그인
3. 한컴독스에 로그인하여 대시보드(`https://www.hancomdocs.com/ko/home`) 또는 에디터(`webhwp.hancomdocs.com`) 탭을 열어둔 상태에서 사용
   - **대시보드**: 파일/폴더 관리, 새 문서 생성 (`/hdocs-dashboard`)
   - **에디터**: 문서 편집 (나머지 skill)

### 실행

```bash
# 프로젝트 디렉토리에서 --chrome 플래그로 실행
claude --chrome
```

`--chrome` 플래그가 Claude in Chrome 확장과의 MCP 연결을 활성화한다. 이 플래그 없이 실행하면 브라우저 자동화 도구를 사용할 수 없다.

## 사용법

### 권장 모델
Opus 사용을 권장하며 해당 모델을 기반으로 테스트 되었음.

### 자연어 요청

에이전트는 자연어 요청을 분석하여 적절한 skill로 자동 라우팅한다:

```
"제목을 입력하고 개요 1 스타일을 적용해줘"
"3행 4열 표를 만들어줘"
"용지를 가로로 바꿔줘"
"보고서 작성해줘"
```

### 슬래시 커맨드

특정 skill을 직접 호출:

| 커맨드 | 기능 | 예시 |
|--------|------|------|
| `/hdocs-dashboard` | 대시보드 파일/폴더 관리 | 파일 이동, 삭제, 폴더 생성 |
| `/hdocs-navigate` | 에디터 탐색/제어, 텍스트 입력 | 탭 찾기, 저장, 텍스트 작성 |
| `/hdocs-style` | F6 스타일 적용/편집 | 개요 적용, 스타일 생성 |
| `/hdocs-table` | 표 생성/편집 | 표 만들기, 셀 편집, 테두리 |
| `/hdocs-format` | 글자/문단 서식 | 글꼴, 크기, 정렬, 줄간격 |
| `/hdocs-page` | 페이지 레이아웃 | 용지 설정, 머리말, 쪽번호 |
| `/hdocs-insert` | 개체 삽입 | 그림, 도형, 수식, 문자표 |
| `/hdocs-write` | 문서 작성 통합 워크플로우 | 보고서 자동 생성 |

## 아키텍처

```
CLAUDE.md (오케스트레이터)
├── 에디터 식별 + 핵심 조작 패턴 26개
├── 서식 원칙 + Skill 라우팅 규칙
│
├── .claude/commands/ (8개 Skill)
│   ├── hdocs-dashboard.md   대시보드 파일 관리
│   ├── hdocs-navigate.md    에디터 탐색/제어
│   ├── hdocs-style.md       F6 스타일 다이얼로그
│   ├── hdocs-table.md       표 생성/편집
│   ├── hdocs-format.md      글자/문단 서식
│   ├── hdocs-page.md        페이지 레이아웃
│   ├── hdocs-insert.md      개체 삽입
│   └── hdocs-write.md       문서 작성 통합
│
├── .claude/settings.local.json  도구 권한 설정
│
└── references/
    └── hdocs-help-urls.md   공식 도움말 URL
```

**동작 흐름**:
1. 사용자 요청 → CLAUDE.md가 적절한 skill로 라우팅
2. Skill이 Claude in Chrome MCP 도구로 브라우저 제어
3. 에디터 UI 요소를 동적 탐색(find/read_page)하여 조작

## 라이선스

GPL-3.0. [LICENSE](LICENSE) 파일 참조.
