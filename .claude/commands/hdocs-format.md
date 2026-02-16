# 한컴독스 글자/문단 서식 (/hdocs-format)

한컴독스 웹 에디터의 글자 모양, 문단 모양, 글머리표/문단번호 서식을 제어하는 skill.

사용자 요청: $ARGUMENTS

---

## 실행 전 절차

1. **에디터 탭 확인**: `tabs_context_mcp` → 한컴독스 에디터 탭 식별
2. **기존 다이얼로그 닫기**: 열린 다이얼로그 있으면 Escape

---

## ★ 서식 원칙: 스타일 우선, 직접 서식은 예외만

### 핵심: 반복되는 서식은 스타일로, 일회성 서식만 직접 적용

스타일이 적합하면 Ctrl+숫자로 적용. 스타일이 부적합하면 **그때** F6에서 편집.
직접 서식(Alt+L, Ctrl+B 등)은 아래 예외 상황에서만 사용한다.

### 직접 서식이 필요한 경우

- 본문 중 특정 단어만 **진하게/기울임**: 텍스트 선택 → Ctrl+B / Ctrl+I
- 특정 텍스트 **글자색**: 선택 → find("글자색") → 색상 선택
- 특정 텍스트 **형광펜**: 선택 → find("형광펜") → 색상 선택
- **자간** 미세 조정: 선택 → Alt+L → 자간 값 입력
- 표 내부 셀 서식: 스타일 적용 불완전 → 직접 크기/정렬 설정

### 스타일 편집 방법 (필요할 때)

스타일 속성이 부적합할 때 F6에서 편집:
```
1. F6 → 편집할 스타일 클릭
2. ⚠ "설정..." 버튼 JS 클릭 (편집(연필)이 아님!)
3. [글자 모양 설정...] → 글꼴, 크기, 진하게 등 → 확인
4. [문단 모양 설정...] → 정렬, 줄간격, 여백 등 → 확인
5. 확인 → 스타일 저장
```
→ 스타일 프리셋 참고값: hdocs-write.md "스타일 프리셋" 섹션
→ "설정..." 버튼 JS 코드: CLAUDE.md 패턴 13 참조

---

## A. 글자 모양 (Alt+L)

### 글자 모양 다이얼로그 열기
```
computer(action="key", text="alt+l", tabId={탭ID})
```
또는 메뉴: 서식 → 글자 모양

### 다이얼로그 내 설정 항목

**기본 탭:**
- 글꼴 (한글/영문/한자 별도 설정 가능)
- 글자 크기
- 장평 (가로 비율, 기본 100%)
- 자간 (글자 간격, 기본 0%)
- 글자 속성: 진하게, 기울임, 밑줄, 취소선, 위첨자, 아래첨자
- 글자색
- 음영색

**확장 탭:**
- 그림자, 양각, 음각 등 효과

### 설정 절차
1. `alt+l`로 다이얼로그 열기
2. `find` 또는 `read_page`로 원하는 입력 필드 찾기
3. `form_input`으로 값 설정 또는 `triple_click` → `type`으로 덮어쓰기
4. 확인 버튼 클릭

### 툴바로 빠른 글자 서식 변경

다이얼로그 없이 툴바에서 직접 변경:

**글꼴 변경:**
```
find(query="글꼴", tabId={탭ID}) → triple_click → type("새 글꼴명") → Enter
```

**글자 크기 변경:**
```
find(query="글자 크기", tabId={탭ID}) → triple_click → type("12") → Enter
```

**진하게 (Bold):**
```
computer(action="key", text="ctrl+b", tabId={탭ID})
또는
find(query="진하게", tabId={탭ID}) → left_click
```

**기울임 (Italic):**
```
computer(action="key", text="ctrl+i", tabId={탭ID})
또는
find(query="기울임", tabId={탭ID}) → left_click
```

**밑줄 (Underline):**
```
computer(action="key", text="ctrl+u", tabId={탭ID})
또는
find(query="밑줄", tabId={탭ID}) → left_click
```

**취소선:**
```
find(query="취소선", tabId={탭ID}) → left_click
```

**글자색:**
```
find(query="글자색", tabId={탭ID}) → left_click → 색상 팔레트에서 선택
```

**형광펜:**
```
find(query="형광펜", tabId={탭ID}) → left_click → 색상 선택
```

---

## B. 문단 모양 (Alt+T)

### 문단 모양 다이얼로그 열기
```
computer(action="key", text="alt+t", tabId={탭ID})
```
또는 메뉴: 서식 → 문단 모양

### 다이얼로그 내 설정 항목

**기본/정렬 탭:**
- 정렬: 양쪽, 왼쪽, 가운데, 오른쪽, 배분, 나눔
- 들여쓰기/내어쓰기
- 왼쪽 여백, 오른쪽 여백
- 첫 줄 들여쓰기

**줄간격 탭:**
- 줄간격 (%, pt, 배수)

**여백 탭:**
- 문단 위/아래 간격

**테두리/배경 탭:**
- 문단 테두리선
- 문단 배경색

### 설정 절차
1. `alt+t`로 다이얼로그 열기
2. 탭 전환 필요시 해당 탭 클릭
3. 입력 필드 찾기 → 값 설정
4. 확인 버튼 클릭

### 툴바로 빠른 문단 서식 변경

**정렬:**
```
양쪽 정렬: find(query="양쪽 정렬", tabId={탭ID}) → left_click
왼쪽 정렬: find(query="왼쪽 정렬", tabId={탭ID}) → left_click
가운데 정렬: find(query="가운데 정렬", tabId={탭ID}) → left_click
오른쪽 정렬: find(query="오른쪽 정렬", tabId={탭ID}) → left_click
배분 정렬: find(query="배분 정렬", tabId={탭ID}) → left_click
```

**줄간격 변경:**
```
find(query="줄간격", tabId={탭ID}) → triple_click → type("160") → Enter
```

---

## C. 글머리표/문단번호

### 글머리표 토글
```
find(query="글머리표 매기기", tabId={탭ID}) → left_click
```
- 클릭하면 현재 문단에 글머리표(□) 적용
- Enter로 다음 문단에도 자동 연속
- 다시 클릭하면 글머리표 해제

### 문단번호 토글
```
find(query="문단 번호 매기기", tabId={탭ID}) → left_click
```
- 클릭하면 현재 문단에 "1." 번호 적용
- Enter로 자동 번호 증가 ("2.", "3.", ...)
- 다시 클릭하면 문단번호 해제

### 문단번호 모양 다이얼로그
```
computer(action="key", text="alt+k", tabId={탭ID})
→ wait(0.3)
→ computer(action="key", text="n", tabId={탭ID})
```
또는 메뉴: 서식 → 문단번호 모양

### 수준 조정
```
수준 증가 (들여쓰기): find(query="수준 증가", tabId={탭ID}) → left_click
  또는: computer(action="key", text="ctrl+Subtract", tabId={탭ID})

수준 감소 (내어쓰기): find(query="수준 감소", tabId={탭ID}) → left_click
  또는: computer(action="key", text="ctrl+Add", tabId={탭ID})
```

---

## D. 모양 복사

다른 텍스트의 서식을 복사하여 적용:

1. 원본 텍스트 선택
2. 모양 복사: `computer(action="key", text="ctrl+alt+c", tabId={탭ID})`
3. 대상 텍스트 선택
4. 붙이기: `computer(action="key", text="ctrl+v", tabId={탭ID})`

**⚠ 주의**: 모양 복사 후 Ctrl+V는 서식만 붙여넣기가 됨(일반 클립보드 붙여넣기가 아님). 모양 복사 모드를 해제하려면 Escape를 누르거나 다른 작업을 수행.

---

## 에러 처리

### 다이얼로그가 안 열림
- 에디터 본문 클릭 → 포커스 확보 → 단축키 재시도
- 메뉴 경로로 대체 시도

### 툴바 버튼을 못 찾음
- `read_page(filter="interactive")`로 전체 툴바 요소 확인
- 다른 쿼리 시도 (예: "Bold" 대신 "진하게", "B" 등)
- screenshot으로 시각적 확인

### 서식이 적용 안 됨
- 텍스트가 제대로 선택되었는지 확인
- 표 안의 텍스트는 셀 선택 상태 확인 필요
- **⚠ 표 헤더 볼드**: Ctrl+B는 Tab으로 다음 셀에 전파되지 않음 → 각 셀마다 Ctrl+B 개별 적용 필요

---

## 참조
- 글자 모양: `format/font/fonts.htm`, `fonts_general.htm`, `fonts_expanded.htm`
- 문단 모양: `format/paragraph/paragraph.htm` 및 하위 페이지
- 도움말 URL 목록: `references/hdocs-help-urls.md`
