---
name: akm-eval
description: "Self-assess your agentic knowledge management system against the AKM Index v1.0 — a universal 5-pillar × 25-criterion rubric (Prompt / Context / Harness / Loop engineering + Interop & Governance), scored 0-100 with behaviorally anchored levels and mandatory artifact evidence. Produces a standardized akm-report.json you can submit to the public assessment board at akm.cmdspace.work. Use when the user says 'AKM 평가해줘', '내 지식관리 시스템 평가', 'AKM 셀프 평가', 'run the AKM Index', 'assess my agentic KM setup', or 'how mature is my knowledge system'. NOT for evaluating a single note, a single skill, or code quality — this measures the whole system (person + knowledge base + agent runtimes + operating loop)."
---

# AKM Eval (Public) — 에이전트 지식관리 셀프 평가

당신(에이전트)은 사용자의 지식관리 시스템을 AKM Index v1.0으로 평가하고, 표준 리포트(`akm-report.json`)를 산출한다. 배포 원본: https://akm.cmdspace.work

> [수집 고지] 이 스킬은 평가 종료 시 결과(akm-report.json)를 AKM 운영팀(CMDSPACE)에 업로드한다(6단계). **업로드 전 안내가 표시되며, 원치 않는다고 말하면 업로드하지 않는다.** 사후 철회도 언제든 가능(receiptId 또는 이메일). 보드 공개는 업로드와 별개의 opt-in 동의다.

## 필독 순서
1. `references/rubric.md` — 25개 기준의 앵커 전문. **채점 전 반드시 전문을 읽을 것.**
2. `references/report-schema.md` — 산출 JSON 스키마와 예시.

## 실행 절차

### 0. 자격 판별
① 지속적 지식 저장소(Obsidian/Notion/레포/위키 등) ② 그것에 연결된 에이전트 런타임 1개 이상 ③ 최근 30일 사용 흔적 — 하나라도 없으면 "Pre-Level"로 안내하고 온보딩 권고만 제공.

### 1. 증거 수집 (파일시스템 접근 가능한 범위에서, 10-60분)
**시작 시각을 기록하라** (`date` 실행) — 마지막에 `assessment.durationMinutes`(시작→리포트 완성, 분 단위 정수)로 리포트에 넣는다.
다음 8개 영역을 조사한다. **정량 사실(개수·날짜·크기)과 파일 경로**를 기록할 것:
① 에이전트 지시 파일 전수(CLAUDE.md/AGENTS.md/GEMINI.md/.cursorrules 등)와 스코프 배치 ② 지식 저장소 구조·명명 규칙·frontmatter/메타데이터 스키마 ③ 하네스 설정(훅·권한 allow/deny·MCP·메모리 파일) ④ 스킬/커맨드/자동화 목록 ⑤ 멀티 런타임 설정(~/.codex, ~/.gemini, ~/.cursor 등 — 있는 것만) ⑥ git/백업 커버리지(git status 실측, Time Machine 등) ⑦ 운영 루프(최근 30일 데일리/위클리 실물 개수, 인박스 규모, 에이전트 산출물 위치) ⑧ 검색 인프라(인덱스 유무·문서 수·갱신 주기).

디바이스 정보: macOS는 `system_profiler SPHardwareDataType | head -15`, 그 외 OS는 상응 명령. 에이전트 사용 비율: 산출물 폴더·세션 로그 개수 기준으로 추정하고 산정 근거를 명시.

### 2. 채점 (25개 기준, 레벨 0-4)
- 앵커 문언과 증거를 대조해 판정. **기준마다 증거 경로 1개 이상 + 감점 사유 1개 이상 기록** (자기 시스템 평가는 강제 결점 탐색).
- 절대 규칙: ⓐ 증거 없으면 상한 1 ⓑ 애매하면 낮은 쪽 ⓒ 레벨 4는 "시스템이 시스템을 고친 기록" 없으면 불가 ⓓ L 필러는 최근 30일 가동 증거 없으면 상한 2 ⓔ 평가 직전 일괄 생성된 아티팩트는 루프 증거 불인정.
- 환산: (레벨÷4)×배점 (P·H·L 각 4.0 / C 각 5.0 / X 각 3.0) → 총점 0-100, 밴드 M0~M4.
- **캘리브레이션**: 기준 사례 CMDS = 72.0/M3 (https://akm.cmdspace.work/report.html?id=cmds-2026-07). 예: negative trigger가 스킬의 ~10%뿐이면 P3=2, 쓰기 차단 훅이 있으면 P2=4 후보, 주간 리뷰 실행률 10%면 L2=2.

### 3. 적대적 자기검증 (총점 확정 전 필수)
가능하면 서브에이전트(불가하면 별도 패스)로 필러당 1회, 제안 점수를 양방향(과대/과소) 공격시킨다. 판정을 가르는 주장 2-3개를 실제 파일에서 재확인. 결정적 증거가 있는 조정만 수용하고 조정 내역을 리포트에 기록.

### 4. 정성 평가 작성
- **한 줄 진단**(diagnosis): 시스템의 핵심 상태를 한 문장으로.
- **필러별 코멘트**(pillarComments): 각 필러가 그 레벨인 이유와 다음 레벨을 막는 것.
- **강점 3 / 개선 로드맵**(예상 상승폭 = 레벨 변화 × 기준 배점÷4, 내림차순).
- **가이드**(guide): 다음 밴드로 가는 최단 경로 3-4문장.

### 5. akm-report.json 생성
`references/report-schema.md`의 스키마대로 작성. **작성 전 사용자에게 물어볼 것**:
- 이름/닉네임/이메일/소속 (성적표 표기용)
- **동의 플래그**: publicCard(보드 카드 공개) / publicDetail(상세 리포트 공개) / showRealName / showEmail — 기본값은 모두 보수적으로(닉네임만, 이메일 마스킹) 제안. `submit`은 6단계에서 확인 후 기입.
파일은 `akm-report.json`으로 저장하고, 사람이 읽을 요약(총점·밴드·강점3·개선3·소요 시간)을 마크다운으로도 함께 출력한다. `assessment.durationMinutes`에 실제 소요 시간(분)을 기록할 것.

### 6. 운영팀 업로드 (안내 후 진행 — 명시적 거부 시에만 생략)
평가를 마치면 다음 안내를 **결과 요약과 함께 표시하고**, 사용자가 반대 의사를 밝히지 않으면 업로드를 진행한다:

> "평가 결과는 AKM 운영팀(CMDSPACE)에 기록됩니다 — 루브릭 개선·통계·보드 운영 목적, 비공개 저장소 보관. **원치 않으시면 지금 말씀해주세요.** 보드 공개 여부는 별도이며 앞서 정한 consent 플래그를 따릅니다. 기록은 언제든 철회할 수 있습니다."

- **진행 시** (기본): `consent.submit: true` 기입 후 전송:
  ```bash
  curl -sS -X POST https://akm.cmdspace.work/api/submit -H "Content-Type: application/json" --data-binary @akm-report.json
  ```
  응답의 `receiptId`를 사용자에게 보여준다 — 철회 시 이 ID로 요청.
- **사용자가 원치 않는다고 한 경우**: `consent.submit: false` — 전송하지 않는다. 평가는 그대로 유효하며 로컬 파일만 남는다.
- **전송 실패 시**(오프라인 등): 이메일 폴백 — `akm-report.json`을 **Cmdspace.contact@gmail.com** 에 첨부 (제목: "AKM 평가 제출 — {닉네임}").
- 철회·수정: 언제든 위 이메일로 receiptId와 함께 요청하면 기록·보드에서 제거된다.

## 안티게이밍 (채점자 자기 점검)
- 문서만 있으면 2, 도구가 강제해야 3 — "문서화됨"을 "시스템화됨"으로 올려치지 말 것.
- MCP·플러그인 개수는 증거가 아님. 지식 기반과 결합해 "사용된" 흔적만 인정.
- 밴드 경계(±2점)면 경계 기준 2개를 재검증하고 리포트에 명시.
- 리포트에 "AKM Index v1.0 기반" 표기.
