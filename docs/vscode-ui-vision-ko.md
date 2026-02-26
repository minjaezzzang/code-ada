# Mu VS Code-Style UI 리디자인 비전 (Korean Draft)

## 1) 목표 정의: “완전히 똑같이”의 현실적 해석

“VS Code와 완전히 동일”을 목표로 하되, **법적/브랜드/기술적 제약** 때문에 100% 복제보다는 아래 원칙으로 접근하는 것이 안전하고 지속 가능하다.

- **경험 동일성(UX parity)**: 사용자가 체감하는 정보 구조, 단축키 흐름, 패널 전환 속도, 테마 일관성을 VS Code 수준으로 맞춘다.
- **브랜드 독립성**: 아이콘/로고/고유 아트워크는 독자 자산을 사용한다.
- **Mu 정체성 보존**: 교육용/초보 친화성은 유지하되, 고급 사용자에게는 VS Code식 확장 인터페이스를 제공한다.

---

## 2) 핵심 UX 북극성 (North Star)

사용자가 Mu를 실행했을 때 다음 느낌이 즉시 들어야 한다.

1. “좌측 액티비티 바 + 사이드바 + 중앙 에디터 + 하단 패널”의 익숙한 구조
2. “Ctrl/Cmd+Shift+P 한 방에 모든 기능 호출”
3. “파일 탐색/검색/런너/깃/디버그가 하나의 워크벤치에서 자연스럽게 이어짐”
4. “테마, 아이콘, 폰트, 간격이 균일하고 빠르다”

---

## 3) 정보 구조(IA) 설계안

### 상단
- **커맨드 센터형 타이틀바** (플랫폼별 네이티브 차이 흡수)
- 메뉴바는 기본 숨김(Alt 노출) 옵션 제공

### 좌측
- **Activity Bar**
  - Explorer
  - Search
  - Source Control
  - Run & Debug
  - Extensions(초기엔 Placeholder 가능)
- 클릭 시 **Primary Sidebar** 전환

### 중앙
- **Editor Group**
  - 탭 고정(Pin), 미리보기 탭(italic), 분할(수직/수평)
  - Breadcrumb + Problem decorations

### 하단
- **Panel**
  - Terminal / Problems / Output / Debug Console
  - 패널 위치 하단↔우측 전환

### 우측(선택)
- Secondary Sidebar (Outline/Docs/Assistant)

### 하단 상태바
- 인터프리터, 인코딩, 줄바꿈, Git branch, 오류/경고 수, 모드 상태 표시

---

## 4) 비주얼 시스템 제안

### 디자인 토큰
- spacing: 4/8/12/16 기반
- radius: 4 (기본), 6(대형)
- typography: 에디터용 monospace + UI sans
- motion: 120ms/180ms ease-out

### 테마 시스템
- 기본 3개: Dark+, Light+, High Contrast
- JSON 기반 테마 오버라이드 지원(장기)
- semantic color token 적용:
  - surface/background
  - border
  - accent
  - error/warn/info
  - editor selection/current line

### 아이콘
- 라이선스 안전한 아이콘 세트 채택
- 파일 아이콘 테마 분리 가능 구조

---

## 5) 상호작용 패턴 (VS Code 감성의 핵심)

1. **Command Palette 우선 철학**
   - 모든 주요 액션은 palette에서 검색 가능
   - fuzzy search + 최근 사용 우선 정렬

2. **Quick Open**
   - `Ctrl/Cmd+P` 파일 점프
   - `@` 심볼 검색, `:` 라인 이동, `#` 워크스페이스 심볼

3. **컨텍스트 메뉴 정합성**
   - 탐색기/에디터 탭/에디터 본문/패널의 메뉴 체계 통일

4. **키보드 우선 운영**
   - 포커스 이동 루프 명확화
   - 단축키 충돌 탐지 및 재바인딩 UI 제공(장기)

---

## 6) 기술 구현 전략 (Mu/Qt 가정)

### A. Workbench Shell 레이어 도입
- 현재 화면을 “단일 편집기”에서 “워크벤치 레이아웃”으로 승격
- Qt의 `QSplitter`, `QDockWidget` 또는 커스텀 레이아웃 매니저로 영역 분리

### B. UI 상태 저장
- 사이드바 열림/닫힘, 패널 높이, 마지막 탭, 분할 상태를 세션 저장

### C. Command Registry
- 모든 UI 액션을 command id로 등록
- 메뉴, 버튼, 단축키, palette가 동일 command를 호출

### D. View Container 플러그형 구조
- Explorer/Search/SCM/Run view를 독립 컴포넌트로 분리
- 장기적으로 Extension-like 주입도 가능하게 인터페이스 정의

### E. 성능
- 대형 파일/폴더에서 lazy loading
- 파일 트리 virtualize
- syntax highlight/diagnostics 비동기 처리

---

## 7) 단계별 로드맵 (현실적)

### Phase 0: UI 토대 정리 (1~2주)
- 디자인 토큰 도입
- 색상/간격/폰트 정규화
- 상태바 재구성

### Phase 1: 레이아웃 VS Code화 (2~4주)
- Activity Bar + Sidebar + Editor + Panel 골격 구현
- 기본 Explorer/Output 연결

### Phase 2: 생산성 핵심 (3~5주)
- Command Palette, Quick Open
- 탭 분할/고정/미리보기 탭
- Problems 패널

### Phase 3: 개발 워크플로우 (3~6주)
- Run/Debug 패널 통합
- SCM 1차(상태 표시 + commit/push)
- 터미널 UX 개선

### Phase 4: 완성도 (지속)
- 접근성(A11y)
- 키맵/테마 커스터마이징
- 플러그인 포인트 정교화

---

## 8) 성공 지표 (KPI)

- 신규 사용자의 첫 실행 후 “편집→실행” 완료 시간
- 단축키 기반 작업 비율
- 패널 전환 평균 시간
- 사용자 설문: “VS Code와 유사한가?” 5점 척도
- 크래시율/프레임 드랍/메모리 사용량

---

## 9) 리스크와 대응

1. **“완전 복제” 기대 vs 현실 격차**
   - 대응: 단계별 공개 + parity checklist 운영
2. **UI 복잡도 상승으로 초보자 이탈**
   - 대응: Beginner Mode(간소 UI) 제공
3. **성능 저하**
   - 대응: feature flag + 프로파일링 우선
4. **라이선스/브랜드 이슈**
   - 대응: 자체 아이콘/문구/자산 사용

---

## 10) 아주 공격적인 상상(차별화 포인트)

- **Mu Mentor Panel**: 코드 목적을 자연어로 설명하면 파일 생성/실행/디버깅 경로를 안내
- **Learning Timeline**: 학생이 어떤 오류를 어떻게 해결했는지 타임라인으로 시각화
- **One-Click Classroom Mode**: 교사용 배포 설정(단축키 제한, 힌트 패널 고정)
- **Device-aware Sidebar**: micro:bit/보드 연결 상태를 Activity Bar에 실시간 표시

> 결론: “VS Code처럼 보이는 Mu”가 아니라, **“VS Code급 생산성과 Mu의 교육 친화성이 결합된 워크벤치”**를 목표로 하면 오래간다.
