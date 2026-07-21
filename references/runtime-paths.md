# 런타임 경로 레지스트리 (Runtime Path Registry)

> 평가 1단계(증거 수집) ⑤ 멀티 런타임 조사에서 참조하는 **탐지 경로 예시 목록**.
> 목적은 하나다: **사용자가 Claude Code 외의 에이전트를 잘 쓰고 있는데, 평가자가 그 경로를 몰라 활동 이력이 점수에 반영되지 않는 사고를 막는 것.**

## 스캔 원칙 (필수)

1. **있는 것만**: 아래 경로는 예시다. 존재하지 않으면 조용히 건너뛴다. 버전에 따라 경로가 다를 수 있으니 "경로가 없음 = 그 런타임 미사용"으로 단정하지 말고, 상위 폴더(`~/.<도구명>/`, `~/.config/<도구명>/`, `~/.local/share/<도구명>/`)를 한 번 더 확인한다.
2. **로컬 읽기 전용**: 스캔은 전부 로컬 작업이며, 파일 **내용**은 리포트에 넣지 않는다. 기록하는 것은 정량 사실 — 개수·타임스탬프·크기·경로 — 뿐이다.
3. **시크릿 비개봉**: `auth.json`, `.env`, `credentials/`, `*.key`, 토큰 파일은 **존재 여부만** 기록하고 열지 않는다 (X4 증거로는 "존재+권한 설정"이면 충분하다).
4. **목록에 없는 런타임**: 같은 요령으로 탐지한다 — 홈 닷폴더 + 프로젝트 닷폴더에서 `sessions/`·`logs/`·`skills/`·`memory/`·설정 파일을 찾으면 된다. 범용 1차 탐지:
   ```bash
   ls -d ~/.*/ ~/.config/*/ ~/.local/share/*/ 2>/dev/null   # 후보 닷폴더 나열
   # 후보 안에서 sessions|skills|logs|memory|agents 하위 폴더가 있으면 에이전트 런타임으로 간주하고 조사
   ```
5. **JSONL만 세지 말 것**: 일부 런타임은 세션 정본이 SQLite다 (Hermes `state.db`, OpenClaw `openclaw-agent.sqlite`, Cursor `state.vscdb`). `sqlite3`가 없거나 파싱이 부담이면 **파일 크기·mtime을 활동 증거로 기록**하면 된다 — 내용 파싱은 필수가 아니다.

## 1. 홈 디렉토리 경로 (사용자 전역 — 세션/로그/이력)

| 런타임 | 세션·로그·이력 | 스킬 / 설정 | 비고 |
|---|---|---|---|
| **Claude Code** | `~/.claude/projects/**/*.jsonl`<br>`~/.claude/history.jsonl`<br>`~/.claude/projects/*/memory/` | `~/.claude/skills/`<br>`~/.claude/settings.json`<br>`~/.claude/CLAUDE.md` | 세션 이력이 가장 명확 |
| **Codex CLI** | `~/.codex/sessions/**/*.jsonl` | `~/.codex/config.toml` | AGENTS.md를 읽음 (X1 증거) |
| **Gemini CLI** | `~/.gemini/` (chats, checkpoints, tmp) | `~/.gemini/settings.json`<br>`~/.gemini/skills/` | GEMINI.md를 읽음 |
| **Grok Build** | `~/.grok/` (debug, memory, logs) | `~/.grok/config.toml`<br>`~/.grok/skills/`<br>`~/.grok/plugins/` | CLAUDE.md·`.claude/`·AGENTS.md 자동 인식 → 그 자체가 X1 어댑터 증거. `auth.json`은 존재만 기록 |
| **Hermes Agent** | `~/.hermes/state.db` (SQLite, **정본**)<br>`~/.hermes/sessions/*.jsonl` (레거시)<br>`~/.hermes/logs/` | `~/.hermes/config.yaml`<br>`~/.hermes/skills/`, `~/.hermes/pending/skills/`<br>`~/.hermes/profiles/<이름>/` | 프로필별로 세션·스킬 분리 — 프로필도 순회할 것. `.env`는 존재만 기록 |
| **OpenClaw** | `~/.openclaw/agents/<id>/agent/openclaw-agent.sqlite` (세션)<br>`~/.openclaw/workspace/memory/*.md` (데일리 로그) | `~/.openclaw/workspace/` (AGENTS.md·MEMORY.md 등)<br>`~/.openclaw/openclaw.json` | Markdown 메모리는 H4·L 증거로 우수. `credentials/`는 절대 열지 말 것 |
| **Gajae-Code (gjc)** | `~/.gjc/` + 프로젝트 `.gjc/rlm/<session>/` (notebook.ipynb, report.md) | `~/.gjc/skills/` | plans·specs·ultragoal 등 런타임 상태는 프로젝트 `.gjc/`에 |
| **OpenCode** | `~/.local/share/opencode/` (storage/session 포함) | `~/.config/opencode/` | AGENTS.md를 읽음 |
| **Goose** | `~/.local/share/goose/sessions/` 또는 `~/.config/goose/sessions/` (플랫폼별 상이) | `~/.config/goose/config.yaml` | |
| **Continue.dev** | `~/.continue/` (sessions 포함) | `~/.continue/config.yaml` | |
| **Cursor** | `~/Library/Application Support/Cursor/User/workspaceStorage/**/state.vscdb` (macOS) | `~/.cursor/` + 프로젝트 `.cursor/` | 파일 투명도가 낮음 — 크기·mtime 기록으로 충분 |
| **Aider** | 프로젝트 `.aider.chat.history.md`, `.aider.input.history` | `.aider.conf.yml` (홈 또는 프로젝트) | 이력이 프로젝트 단위 |
| **Amp** | `~/.config/amp/` (threads 관련) | `~/.config/amp/skills/` 또는 `~/.agents/skills/` | AGENTS.md를 읽음 |
| **Pi Agent** | `~/.pi/agent/sessions/` | `~/.pi/agent/` | |
| **Kilo Code** | `~/.config/kilo/` | 프로젝트 `.kilo/` | |
| **공통·신흥 표준** | — | `~/.agents/`, `~/.agents/skills/` | 여러 도구가 공유 시도 중 — 발견 시 어느 런타임이 참조하는지 확인 |

## 2. 프로젝트 레벨 경로 (레포 안)

```text
.claude/                  # Claude Code
.codex/                   # Codex CLI (있는 경우)
.gemini/                  # Gemini
.grok/                    # Grok Build (skills, MCP, 권한 규칙)
.gjc/                     # Gajae-Code (rlm 세션, plans, specs)
.cursor/                  # Cursor (rules 등)
.opencode/                # OpenCode
.windsurf/  /  .devin/    # Windsurf / Devin
.clinerules/  /  .roo/    # Cline / Roo
.kilo/                    # Kilo
.agents/                  # 공통 시도
.aider.chat.history.md    # Aider (파일)
AGENTS.md / CLAUDE.md / GEMINI.md / AGENT.md   # 지시 파일 (아티팩트 #1)
```

프로젝트 스캔 범위는 **시스템 경계 선언에 포함된 레포**만이다 (rubric §8.1). 경계 밖 레포를 뒤지지 않는다.

## 3. 우선 스캔 경로 (고신호 순)

```text
# 세션·트랜스크립트 (활동 기간·빈도 — L 필러, 에이전트 사용 비율)
~/.claude/projects/**/*.jsonl
~/.codex/sessions/**/*.jsonl
~/.hermes/state.db  (+ ~/.hermes/sessions/** 레거시)
~/.openclaw/agents/**/openclaw-agent.sqlite
~/.gjc/**  및  <프로젝트>/.gjc/rlm/**
~/.local/share/opencode/**
~/.local/share/goose/sessions/**  (또는 ~/.config/goose/sessions/**)
<프로젝트>/.aider.chat.history.md

# 설정 + 스킬 (메타 정보 — P·H·X 필러)
~/.claude/   ~/.codex/   ~/.gemini/   ~/.grok/   ~/.hermes/   ~/.openclaw/
~/.agents/   ~/.config/opencode/   ~/.config/goose/   ~/.continue/
~/.cursor/   ~/.pi/agent/   ~/.config/amp/   ~/.config/kilo/
```

## 4. 추출 항목 → 채점 반영

| 발견물 | 추출할 정량 사실 | 반영처 |
|---|---|---|
| `*.jsonl` 세션 | 파일 수, 최초·최근 타임스탬프, (가능하면) 턴 수·도구 호출 여부 | L 필러 30일 가동 증거, `environment` 에이전트 사용 비율 |
| SQLite 세션 (`state.db` 등) | 파일 크기, mtime (파싱은 선택) | 상동 |
| `sessions/`·`logs/`·`memory/` | 항목 수, 날짜 분포 | L 필러, H4 |
| `skills/`·`plugins/`·커맨드 | 개수, 목록, 도메인/범용 구분 | P2·P3, H2 |
| 설정 파일 (settings/config) | 모델, 권한 allow/deny, 훅, MCP 목록 | H1, H3 |
| 런타임별 지시 파일 연결 (CLAUDE.md/AGENTS.md/GEMINI.md 심링크·자동 인식 포함) | 어떤 런타임이 어떤 정본을 읽는가 | **X1, X2, 아티팩트 #1·#10** |
| 시크릿 파일 | 존재 여부·위치만 | X4 인벤토리 |

발견된 모든 런타임은 **아티팩트 #10 (런타임 목록 + 컨텍스트 연결 상태)** 에 기재하고, 주력 외 런타임은 X1의 v1.1 각주("검증된 이식성" — 어댑터 문서화 + 실행 검증 기록 1회 이상)를 적용해 판정한다.

## 참고 문서

- Claude Code — https://code.claude.com/docs/en/claude-directory
- Grok Build — https://docs.x.ai/build/overview
- Hermes Agent — https://hermes-agent.nousresearch.com/docs/user-guide/configuration
- OpenClaw — https://docs.openclaw.ai/concepts/agent-workspace
- Codex CLI — https://github.com/openai/codex
- Gemini CLI — https://geminicli.com/docs/reference/configuration/
- Gajae-Code — https://github.com/Yeachan-Heo/gajae-code
- Amp — https://ampcode.com/manual
- Pi Agent — https://github.com/badlogic/pi-mono

> 이 목록은 스냅샷이다 (2026-07 기준). 경로가 실물과 다르면 실물을 우선하고, 새 런타임을 발견하면 이 파일에 추가하는 PR을 환영한다.
