# Web Serial Terminal — Project Context for Claude Code

## 나에 대해
- **직무**: Embedded System Software 개발자
- **주요 경험**: Linux kernel, device driver, Android framework/HAL, bash/build script, crash log 분석
- **웹 개발 경험**: 거의 없음 (JS/TS 초보, 프레임워크 경험 없음)
- **기존 작업**: WPF + .NET 8 기반 Windows용 UART 디버그 로그 뷰어 MVP 제작 경험 있음

## 이 프로젝트를 만드는 이유
- 기존 WPF 버전은 팀 배포 시 .NET runtime + DLL 동봉이 필요해 번거로움
- 웹 방식으로 URL 하나만 공유하면 팀원 누구나 바로 사용 가능하게 하고 싶음
- 팀원들이 현재 PuTTY / MobaXterm을 serial 터미널로 사용 중인데, 두 툴 모두 keyword filtering 기능이 없거나 불편함
- 팀원들이 필요한 기능을 직접 추가할 수 있도록 웹 기반으로 협업하기 쉽게 만들고 싶음

## 타겟 사용자 환경
- 임베디드 개발팀 (팀원 모두 Windows + Chrome/Edge 환경)
- 현재 PuTTY, MobaXterm으로 serial port 연결해서 UART 로그 확인 중
- 빠르게 쏟아지는 serial 로그에서 원하는 라인만 빠르게 필터링하는 기능이 핵심 니즈

## 핵심 기능 (MVP)

### 반드시 있어야 하는 것
- Serial port 선택 / baud rate 설정 / 연결 / 해제
- 수신 로그 표시 (메인 뷰, 실시간 스크롤)
- Keyword 등록 (여러 개, 색상 구분)
- Keyword 매칭 라인 → Filtered View (별도 창/패널)에 실시간 표시
- Keyword별 on/off 토글

### v2에서 추가할 것 (MVP에서는 구현 안 해도 됨)
- 로그 파일 저장 (.txt / .log)
- 타임스탬프 표시 옵션
- Keyword별 매칭 카운터
- Baud rate 커스텀 입력

## 기술 스택

### 핵심
- **Web Serial API** — 브라우저에서 직접 serial port 제어 (Chrome/Edge 89+만 지원)
- **Vanilla HTML + CSS + JavaScript** — 프레임워크 없이 단일 파일로 구성
  - 이유: 웹 초보자도 코드 전체를 이해하고 수정할 수 있도록
  - 이유: 빌드 도구 없이 파일 하나로 팀원에게 공유 가능

### 구조
```
web-serial-terminal/
├── index.html       ← 전체 앱 (HTML + CSS + JS 단일 파일)
└── README.md        ← 사용법 (포트 선택 방법 등)
```

### UI 레이아웃
```
┌─────────────────────────────────────────────┐
│  [Port 선택] [BaudRate ▼] [Connect] [Clear] │  ← 상단 툴바
│  Keywords: [input + 색상] [Add] [keyword1 ×] [keyword2 ×] │
├────────────────────┬────────────────────────┤
│                    │                        │
│   Main Log View    │   Filtered View        │
│   (전체 로그)       │   (keyword 매칭 라인)  │
│                    │                        │
└────────────────────┴────────────────────────┘
```

## 코드 스타일 지침
- 모르는 개념이 나오면 반드시 주석으로 설명 추가
- 왜 이 방식을 선택했는지 주요 결정 지점에 주석으로 남길 것
- 복잡한 구조보다 단순하고 읽기 쉬운 코드 우선
- 함수는 하나의 역할만 담당하도록 작게 유지

## 주요 기술 제약 사항 (Web Serial API)
- HTTPS 또는 localhost에서만 동작
- 사용자가 매번 포트 선택 다이얼로그에서 직접 선택해야 함 (자동 연결 불가, 보안 정책)
- Chrome / Edge만 지원, Firefox / Safari 미지원
- 로그가 많아질 경우 메모리 관리 필요 → 최근 N줄만 유지하는 버퍼 방식 사용

## 메모리 관리 전략
- 메인 뷰 버퍼: 최대 10,000줄 유지 (초과 시 오래된 것부터 제거)
- Filtered 뷰 버퍼: 최대 5,000줄 유지
- DOM 직접 조작 최소화 (innerHTML += 방식 금지, DocumentFragment 사용)
- 자동 스크롤: 스크롤이 맨 아래에 있을 때만 자동 스크롤, 위로 올려보는 중이면 멈춤

## 시작 명령어
```bash
# 빌드 불필요, 브라우저에서 바로 열면 됨
# 단, Web Serial API는 file:// 프로토콜에서 동작 안 할 수 있으므로 로컬 서버 권장
python -m http.server 8080
# 또는
npx serve .
```

## 작업 지침
- 새 기능 추가 전 반드시 현재 코드 구조 파악 후 진행
- Web Serial API 관련 에러는 반드시 사용자에게 명확한 메시지로 표시
- Chrome/Edge가 아닌 브라우저에서 접근 시 안내 메시지 표시
