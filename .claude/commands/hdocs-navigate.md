# 한컴독스 에디터 탐색/제어 (/hdocs-navigate)

한컴독스 웹 에디터의 기본 탐색, 제어, 텍스트 입력을 담당하는 기반 skill.
다른 모든 hdocs skill이 이 skill의 패턴에 의존한다.

사용자 요청: $ARGUMENTS

---

## 실행 전 필수 절차

### 1단계: 에디터 탭 식별
```
tabs_context_mcp → 탭 목록 확인
URL에 "webhwp.hancomdocs.com" 포함된 탭 찾기
```
- 에디터 탭이 없으면: "한컴독스 에디터를 브라우저에서 먼저 열어주세요" 안내
- 여러 개면: 사용자에게 어떤 문서를 대상으로 할지 확인

### 2단계: 에디터 상태 확인
```
screenshot → 에디터 현재 상태 시각적 확인
```
- 편집 모드인지 확인 (메뉴바, 도구상자 표시 여부)
- 다이얼로그가 열려있으면 Escape로 먼저 닫기
- **⚠ 문단부호/격자 활성화 필수**: 보기 메뉴 → 표시/숨기기 → 문단 부호, 격자 보기를 **반드시** 활성화. 비활성이면 작업 시작 전 반드시 켜기 (좌표 정확도에 직접 영향)

---

## 핵심 기능

### A. 텍스트 입력 (글쓰기)

에디터 본문에 텍스트를 입력하는 가장 기본적인 작업.

**기본 텍스트 입력:**
1. 에디터 본문 영역 클릭 (캔버스에 포커스 확보)
   - `find(query="편집 영역")` 또는 에디터 중앙부 좌표 클릭
2. `computer(action="type", text="입력할 텍스트")` 로 텍스트 입력
3. 줄바꿈이 필요하면 `computer(action="key", text="Enter")`

**단락 단위 입력 (권장 패턴):**
```
1. 첫 번째 단락 type
2. Enter 키
3. 두 번째 단락 type
4. Enter 키
... 반복
```

**주의사항:**
- 한글 입력 시 IME 조합 중일 수 있음 → 다음 동작 전 짧은 대기(`wait 0.3`) 권장
- 매우 긴 텍스트는 500자 이내 단위로 나누어 입력
- 특수문자가 포함된 경우 `computer(action="key")` 사용 고려
- 입력 전 커서 위치가 올바른지 screenshot으로 확인

**커서 이동:**
- 방향키: `computer(action="key", text="ArrowUp/ArrowDown/ArrowLeft/ArrowRight")`
- 문서 처음: `computer(action="key", text="ctrl+Home")`
- 문서 끝: `computer(action="key", text="ctrl+End")`
- 줄 처음: `computer(action="key", text="Home")`
- 줄 끝: `computer(action="key", text="End")`
- 단어 단위: `computer(action="key", text="ctrl+ArrowLeft/ArrowRight")`
- **⚠ Page_Down/Page_Up 사용 금지**: 이 키들은 HWP 웹 에디터에서 **텍스트로 입력됨** (페이지 이동 아님!)

**페이지/뷰포트 이동 (커서 이동 아님):**
- 스크롤: `computer(action="scroll", direction="down/up", scroll_amount=5~10)` → 뷰포트만 이동
- 특정 텍스트 위치로 점프: `Ctrl+F` → 찾기 → 해당 위치로 이동 (**⚠ 표 내부 텍스트는 못 찾는 HWP 버그 있음**)
- 문서 처음/끝으로: `Ctrl+Home` / `Ctrl+End` (포커스 확보 후)

**텍스트 선택:**
- 전체 선택: `computer(action="key", text="ctrl+a")`
- Shift + 방향키: 선택 영역 확장
- Shift+Home/End: 줄 시작/끝까지 선택
- Ctrl+Shift+Home/End: 문서 시작/끝까지 선택

**텍스트 삭제:**
- `Backspace`: 커서 앞 글자 삭제
- `Delete`: 커서 뒤 글자 삭제
- 선택 후 `Delete` 또는 `Backspace`: 선택 영역 삭제

### B. 새 문서 생성

**방법 1 - 메뉴:**
1. `find(query="파일 메뉴")` → 클릭
2. `find(query="새 문서")` 또는 `find(query="새문서")` → 클릭

**방법 2 - 한컴독스 홈에서:**
1. 한컴독스 홈(docs.hancom.com) 탭에서 새 문서 버튼 찾기

### C. 문서 저장

**방법 1 - 단축키:**
```
computer(action="key", text="ctrl+s")
```

**방법 2 - 메뉴:**
1. `find(query="파일 메뉴")` → 클릭
2. `find(query="저장")` → 클릭

**다른 이름으로 저장:**
1. `find(query="파일 메뉴")` → 클릭
2. `find(query="다른 이름으로 저장")` → 클릭
3. 파일명 입력 필드 → `form_input` 또는 `type`
4. 확인 버튼 클릭

### D. 문서 다운로드

1. `find(query="파일 메뉴")` → 클릭
2. `find(query="다운로드")` → 클릭
3. 형식 선택 다이얼로그가 나타나면 원하는 형식 선택
4. **주의**: 다운로드는 사용자 확인 필요 (explicit permission)

### E. 메뉴 열기/닫기

**메뉴 열기:**
```
find(query="{메뉴명} 메뉴") → 클릭
예: find(query="서식 메뉴"), find(query="표 메뉴")
```

**하위 메뉴 선택:**
```
메뉴 열린 상태에서:
find(query="{항목명}") → 클릭
```

**메뉴 닫기:**
```
computer(action="key", text="Escape")
```

### F. 단축키 실행

한컴독스 웹에서 단축키 전송:
```
computer(action="key", text="{단축키}")
```

**주요 단축키:**
| 작업 | 단축키 |
|------|--------|
| 되돌리기 | `ctrl+z` |
| 다시 실행 | `ctrl+shift+z` |
| 복사 | `ctrl+c` |
| 붙여넣기 | `ctrl+v` |
| 오려두기 | `ctrl+x` |
| 모두 선택 | `ctrl+a` |
| 저장 | `ctrl+s` |
| 찾기 | `ctrl+f` |
| 글자 모양 | `alt+l` |
| 문단 모양 | `alt+t` |
| 스타일 | `F6` |
| 편집 용지 | `F7` |
| 표 만들기 | `alt+n t` |
| 쪽 나누기 | `ctrl+Enter` |
| 문서 처음 | `ctrl+Home` |
| 문서 끝 | `ctrl+End` |
| 찾아가기 | `alt+g` |

**⚠ 사용 금지 키:**
| 키 | 이유 |
|----|------|
| `Page_Down` / `Page_Up` | **텍스트로 입력됨** (문서 훼손!) |
| `F5` | 브라우저 새로고침 |
| `Ctrl+L` | 브라우저 주소창 |
| `Ctrl+T` / `Ctrl+W` / `Ctrl+N` | 브라우저 탭/창 조작 |

**복합 단축키 (순차 키 입력):**
Alt+N+T 같은 순차 키는 각 키를 순서대로 전송:
```
computer(action="key", text="alt+n") → wait(0.3) → computer(action="key", text="t")
```

### G. 되돌리기/다시실행

```
되돌리기: computer(action="key", text="ctrl+z")
다시실행: computer(action="key", text="ctrl+shift+z")
```

### H. 찾기 (Ctrl+F)

**⚠ Ctrl+F는 브라우저 찾기가 아닌 HWP 자체 찾기 다이얼로그를 연다.**

1. `computer(action="key", text="ctrl+f")` → 찾기 다이얼로그 열기
2. **⚠ 찾을 방향 필수 사전 설정** (검색어 입력 전에!):
   - 기본값 "아래로" → 커서 뒤쪽만 검색 (커서 위 내용은 못 찾음!)
   - **권장: "문서 전체" 라디오 클릭** → 전체 문서에서 검색
   ```
   find(query="문서 전체") → left_click
   ```
3. 다이얼로그 구조:
   - "찾을 내용" 입력 필드
   - "찾을 방향": 아래로(기본) / 위로 / 문서 전체 (라디오 버튼)
   - "다음 찾기" 버튼, "닫기" 버튼
4. **⚠ 필드 값 입력 (안전한 방법):**
   ```
   find(query="찾을 내용") → left_click
   computer(action="key", text="Home")
   computer(action="key", text="shift+End")   ← 기존 값 전체 선택
   computer(action="type", text="검색어")      ← 선택 영역 덮어쓰기
   ```
   - **⚠ triple_click+type 금지**: 기존 값에 추가됨 (대체 아님)
   - **⚠ Ctrl+A 금지**: 에디터 본문 전체가 선택되어 문서 파괴 위험
   - **⚠ form_input 주의**: DOM만 변경, HWP 내부 상태 미반영 가능
4. `find(query="다음 찾기")` → 클릭
5. 닫기: `find(query="닫기")` 클릭 또는 Escape

### I. 찾아 바꾸기 (Ctrl+H)

1. `computer(action="key", text="ctrl+h")` → 찾아 바꾸기 다이얼로그 열기
2. 다이얼로그 구조:
   - "찾을 내용" 입력 필드
   - "바꿀 내용" 입력 필드
   - "찾을 방향": 아래로(기본) / 위로 / 문서 전체
   - 버튼: "바꾸기", "다음 찾기", "모두 바꾸기", "닫기"
3. **⚠ 각 필드 값 입력 시 안전한 방법 (H절과 동일):**
   ```
   find(query="찾을 내용") → left_click → Home → Shift+End → type("찾을 텍스트")
   find(query="바꿀 내용") → left_click → Home → Shift+End → type("바꿀 텍스트")
   ```
4. 바꾸기 또는 모두 바꾸기 클릭

### J. 찾아가기 (Alt+G)

특정 쪽, 구역, 줄, 스타일, 조판 부호, 책갈피로 빠르게 이동:
1. `computer(action="key", text="alt+g")` → 찾아가기 다이얼로그 열기
2. 라디오 버튼으로 이동 대상 선택: 쪽 / 구역 / 줄 / 스타일 / 조판 부호 / 책갈피
3. 이동할 번호 입력
4. "가기" 버튼 클릭
5. **⚠ Alt+G+T 등 순차키 조합은 불가**: Alt+G가 찾아가기로 먼저 잡힘

---

## 에러 복구 패턴

### 포커스 분실
```
1. 에디터 캔버스 영역 클릭 (본문 중앙부)
2. screenshot으로 포커스 확인
3. 재시도
```

### 다이얼로그 잔여
```
1. computer(action="key", text="Escape")
2. 필요시 여러 번 Escape
3. screenshot으로 상태 확인
```

### 메뉴가 닫히지 않음
```
1. 에디터 본문 영역 아무 곳 클릭
2. Escape 키
```

---

## 참조
- 공식 도움말: `references/hdocs-help-urls.md`
- 상위 오케스트레이터: `CLAUDE.md`
