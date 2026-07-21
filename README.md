# AKM Eval — 에이전트 지식관리 셀프 평가 스킬

> **AKM Index v1.1**으로 "내 지식관리 시스템이 AI 에이전트와 함께 얼마나 성숙하게 굴러가는가"를 0–100점으로 측정하는 Claude Code 스킬.
> 공식 사이트: **https://akm.cmdspace.work** · 참여 안내: **https://akm.cmdspace.work/submit.html**

[![AKM Index](https://img.shields.io/badge/AKM%20Index-v1.1-134538)](https://akm.cmdspace.work)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## 무엇을 측정하나

노트 정리 실력이 아니라 **시스템 전체** — 사람 + 지식 저장소 + 에이전트 런타임 + 운영 루프 — 를 측정합니다. 아름다운 제텔카스텐도 에이전트가 접근 못 하면 낮은 점수를, 노트가 거칠어도 에이전트 파이프라인이 견고하면 높은 점수를 받습니다.

| 필러 | 핵심 질문 | 가중치 |
|---|---|---|
| **P** Prompt Engineering | 에이전트에게 주는 지시가 버전 관리되는 자산인가? | 20% |
| **C** Context Engineering | 맞는 지식을 적정 분량으로 찾을 수 있는가? | 25% |
| **H** Harness Engineering | 도구·권한·메모리·오케스트레이션이 정비됐는가? | 20% |
| **L** Loop Engineering | 사이클이 실제로 돌고 복리로 쌓이는가? (30일 가동 증거 필수) | 20% |
| **X** Interop & Governance | 여러 런타임에서 이식되고, 복구·보안이 담보되는가? | 15% |

- 필러당 5개 기준 × 레벨 0–4 (`부재 → 임시 → 문서화 → 시스템화 → 자가개선`) = **25개 기준, 100점 만점**
- 밴드: 0–20 **M0 수동** · 21–40 **M1 도구화** · 41–60 **M2 체계화** · 61–80 **M3 오케스트레이션** · 81–100 **M4 복리형**
- **모든 점수는 증거(파일 경로·설정·날짜 있는 로그)를 요구**합니다. 자기보고만으로는 상한 1점, 레벨 4는 "시스템이 시스템을 고친 기록" 없이는 불가.

## 무엇을 받게 되나

1. **토익 성적표식 평가 리포트** — 총점·밴드, 필러별 점수+정성 코멘트, 25기준 히트맵, 환경·에이전트 사용 지표, 강점 3 / 개선 로드맵(예상 상승폭 순), 가이드 문구
2. (제출 시) **로컬 성적표 HTML** (`akm-report.html`) — 공식 성적표와 동일한 포맷, 브라우저로 열면 끝(서버 불필요). 파일 하나로 친구에게 공유할 수 있고, 받은 사람도 하단 링크에서 바로 자기 평가를 시작할 수 있습니다. 보드 공개를 원치 않아도 제출만 하면 발급됩니다. Claude Code처럼 Artifact를 지원하는 런타임에서는 같은 성적표가 **비공개 웹페이지(Artifact)로도 즉시 게시**되어 링크로 볼 수 있습니다.
3. (제출 + 보드 공개 동의 시) **공개 평가 보드 카드** — https://akm.cmdspace.work/#board — 와 개인 성적표 URL

캘리브레이션 기준 사례(제작자 본인 시스템, 72.0/M3): https://akm.cmdspace.work/report.html?id=cmds-2026-07

## 요구사항

- **Claude Code** (권장 — 다른 코딩 에이전트도 SKILL.md를 지침서로 읽고 수동 실행 가능)
- 지속적으로 쓰는 지식 저장소 (Obsidian, Notion 내보내기, 마크다운 레포, 위키 등 형식 무관)
- 최근 30일 내 에이전트 사용 흔적

## 설치

```bash
# 방법 1 — git clone (권장, 업데이트 쉬움)
git clone https://github.com/johnfkoo951/akm-eval.git ~/.claude/skills/akm-eval

# 방법 2 — ZIP
curl -L https://akm.cmdspace.work/files/akm-eval-skill.zip -o /tmp/akm.zip && unzip -o /tmp/akm.zip -d ~/.claude/skills/
```

## 실행

새 세션을 시작하고 한 마디 — 설치까지 포함해 명령 세 줄이 전부입니다:

```
claude          # 새 세션 시작
AKM 평가해줘     # or: "run the AKM Index"
```

에이전트가 다음을 수행합니다 (10–60분):
`① 자격 판별 → ② 8개 영역 증거 수집 → ③ 25기준 채점(보수 판정) → ④ 적대적 자기검증 → ⑤ 정성 평가 → ⑥ akm-report.json 생성 → ⑦ 업로드 안내`

## 데이터 수집·프라이버시

- 평가 종료 시 결과(`akm-report.json`)가 AKM 운영팀(CMDSPACE)에 업로드됩니다 — **업로드 전 안내가 표시되며, 원치 않는다고 말하면 업로드하지 않습니다.** 평가 자체는 로컬에서 완결됩니다.
- 업로드 시 **영수증(receiptId)** 이 발급되며, 이메일(Cmdspace.contact@gmail.com)로 언제든 철회할 수 있습니다.
- **보드 공개는 업로드와 별개의 opt-in**입니다. 실명/이메일/상세 리포트 공개 여부를 consent 플래그로 직접 선택하며, 기본값은 전부 비공개(닉네임만, 이메일 마스킹)입니다.
- 수신된 데이터는 비공개 저장소에 보관되며 루브릭 개선·통계·보드 운영 목적으로만 사용됩니다.

## 저장소 구조

```
akm-eval/
├── SKILL.md                      # 평가 실행 절차 (에이전트용)
└── references/
    ├── rubric.md                 # AKM Index v1.1 루브릭 전문 (한국어)
    ├── rubric-en.md              # 영문판
    └── report-schema.md          # akm-report.json 스키마 v1.0 + 제출 API
```

## 스키마 (요약)

산출물 `akm-report.json` = 신원·동의 플래그 + 환경(디바이스·런타임 사용 비율·정량 지표) + 25기준 점수 + 정성 평가(진단·필러 코멘트·강점·로드맵·가이드). 전문: [`references/report-schema.md`](references/report-schema.md)

제출 API: `POST https://akm.cmdspace.work/api/submit` — consent.submit=true 필수, 300KB 이하, 스키마 검증 통과 시 receiptId 반환.

## English Summary

**AKM Eval** self-assesses your agentic knowledge management system against the **AKM Index v1.1** — 5 pillars × 25 criteria, 0–100 with behaviorally anchored levels; every point requires artifact evidence. Install with `git clone https://github.com/johnfkoo951/akm-eval.git ~/.claude/skills/akm-eval`, then say **"run the AKM Index"** in a fresh Claude Code session. At the end you'll be notified before results are uploaded to the operations team — nothing is sent if you object, and board publication is a separate opt-in with consent flags. Full English rubric: [`references/rubric-en.md`](references/rubric-en.md). Calibration baseline (author's own system, scored honestly at 72.0/M3): [report](https://akm.cmdspace.work/report.html?id=cmds-2026-07).

## 라이선스·표기

MIT. 루브릭을 인용·개작해 쓸 때 **"Based on AKM Index v1.1 (akm.cmdspace.work)"** 표기를 권장합니다.

만든 사람: [구요한 (Yohan Koo)](https://cmdspace.work) — CMDSPACE. 문의: Cmdspace.contact@gmail.com
