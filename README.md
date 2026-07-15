# 노코드 자동화 미션 결과 보고서

---

## 프로젝트 1: 자동화 도구 비교 구현 (Make vs Zapier)

워크플로우: "Google Form 제출 → 조건 분기(점수 80점 기준) → Google Sheets 기록 + Slack 알림"

### [Make 구현]
- Trigger: Google Forms – Watch Responses (신규 응답 감지)
- Router: 점수 80점 이상(합격) / 미만(불합격) 분기
- Action 1: Google Sheets – Add a Row (합격/불합격 시트에 이름·점수·타임스탬프 기록)
- Action 2: Slack – Send a Message (합격: 🎉 축하 알림 / 불합격: 안내 메시지)

구현 과정 요약:
1) Google 계정 연결 후 Watch Responses 모듈로 대상 폼 지정
2) Router 추가 후 각 Route에 점수 조건 필터 설정
3) 각 Route 끝에 Sheets → Slack 모듈 순서대로 연결
4) 테스트 응답 2건(85점, 60점) 제출하여 양쪽 경로 실행 확인

[📸 Make 시나리오 캔버스 캡처]
[📸 Make 실행 히스토리 캡처]

### [Zapier 구현]
- Trigger: Google Forms – New Form Response (동일 폼 연결)
- Paths: Path A(점수 80 이상) / Path B(80 미만) 분기
- Action 1: Google Sheets – Create Spreadsheet Row (경로별 시트 기록)
- Action 2: Slack – Send Channel Message (경로별 메시지 발송)

구현 과정 요약:
1) Zap 생성 후 Trigger에서 폼 연결 및 샘플 데이터 테스트
2) Paths로 점수 조건 분기 구성 (무료 플랜 제약 시 Filter로 대체)
3) 각 Path에 Sheets → Slack 액션 추가 후 개별 테스트
4) Zap 활성화 후 실제 응답 2건으로 동작 검증

[📸 Zapier 에디터 화면 캡처]
[📸 Zapier Task History 캡처]

### 분기별 실행 결과 증빙

| 시나리오 | 입력 | 결과 | 증빙 |
|---|---|---|---|
| 합격 경로 | 85점 응답 | 합격 시트 기록 + 합격 Slack 알림 ✅ | [📸 캡처] |
| 불합격 경로 | 60점 응답 | 불합격 시트 기록 + 안내 Slack 알림 ✅ | [📸 캡처] |

### 비교 항목

| # | 비교 항목 | Make | Zapier |
|---|---|---|---|
| 1 | UI/UX | 노드 기반 비주얼 캔버스, 전체 흐름 한눈에 파악 | 리스트 기반 단계별 구조, 직선적 |
| 2 | 설정 난이도 | 중급 – 데이터 매핑 개념 이해 필요 | 초급 – 가이드형 UI로 매우 직관적 |
| 3 | 조건 분기 | Router로 다중 분기 자유롭게 구성 | Filter는 쉬우나 Paths는 플랜 제약 존재 |
| 4 | 무료 플랜 | 월 1,000 Ops (분기·다단계에 유리) | 월 100 Tasks (테스트용으로 빠듯함) |
| 5 | 실행 로그 | 실행 히스토리에서 단계별 입출력 데이터 상세 확인 | Task History 제공, 단계별 데이터는 제한적 |
| 6 | 연동 서비스 범위 | 2,000+ 앱, HTTP/Webhook 모듈 강력 | 7,000+ 앱, 커넥터 수 우위 |
| 7 | 오류 처리 | Error Handler로 재시도·대체 경로 설계 가능 | 자동 재시도 있으나 커스텀 설계 제한적 |

### 장단점 및 적합 상황

**Make**
- 장점: 시각적 흐름 파악 용이, 무료 Ops 넉넉, 다중 분기·오류 처리 유연
- 단점: 초기 학습 곡선, 데이터 구조(Bundle) 이해 필요
- 적합 상황: 분기가 많고 복잡한 로직, 오류 처리까지 설계하는 워크플로우

**Zapier**
- 장점: 압도적으로 쉬운 설정, 연동 앱 수 최다, 빠른 프로토타이핑
- 단점: 무료 Task 수 적음, 복잡한 분기는 유료 플랜 필요
- 적합 상황: 단순 선형 자동화를 빠르게 구축할 때, 비개발 직군의 첫 자동화

---

## 프로젝트 2: 자유 주제 – 팀 회의록 자동 정리 및 알림

반복 업무 정의: 매주 회의 후 Google Docs에 회의록을 작성하면 담당자가 수동으로 핵심 내용을 요약해 Slack 채널에 공유하고 이력 시트에 기록하는 작업 (주당 약 30분 소요)

선정 도구: Make (시각적 분기 설계가 직관적이고 무료 1,000 Ops 범위 내 구현 가능)

워크플로우 흐름:
1) Trigger: Google Docs – Watch Documents (새 회의록 문서 감지)
2) Action 1: 텍스트 파싱 (제목/참석자/결정사항 추출)
3) Filter: 결정사항이 존재하는 경우에만 다음 단계로
4) Action 2: Slack – Send a Message (요약 내용 + 담당자 멘션)
5) Action 3: Google Sheets – Add a Row (회의록 이력 기록)

Filter 분기 실행 증빙:
| 시나리오 | 입력 | 결과 | 증빙 |
|---|---|---|---|
| 결정사항 있음 | 결정사항 포함 회의록 | Slack 공유 + 시트 기록 ✅ | [📸 캡처] |
| 결정사항 없음 | 결정사항 미포함 회의록 | Filter에서 중단(미공유) ✅ | [📸 캡처] |

[📸 Make 시나리오 구성 캡처]
[📸 실행 결과(Slack 메시지 + 시트 기록) 캡처]

---

## 보너스 과제

### 보너스 1 – AI 연동 Action
- OpenAI 모듈을 Action으로 추가하여 회의록 결정사항을 자동 요약·분류
- 프롬프트: "다음 회의록에서 결정사항과 담당자를 3줄로 요약해줘"

[📸 AI 모듈 설정 및 출력 결과 캡처]

### 보너스 2 – 실패 알림 및 재시도
- Error Handler 설정: Sheets 기록 실패 시 관리자에게 이메일 알림 발송
- 대체 경로: 저장 실패 시 임시 시트(backup_log)에 적재 후 재시도

[📸 Error Handler 구성 캡처]

---

## 보안 처리 내역
- 모든 스크린샷에서 API Key·토큰은 `***` 마스킹 처리
- 계정 이메일은 일부 가림 처리 (예: ab***@gmail.com)
- Webhook URL은 뒷부분 마스킹 처리

## 과금 리스크 정리
- 두 프로젝트 모두 무료 플랜 범위 내 구현 (Make 1,000 Ops / Zapier 100 Tasks)
- Zapier Paths가 유료인 경우 Filter 2개 Zap으로 무료 대체 구현