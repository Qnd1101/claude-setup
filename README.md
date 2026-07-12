# Claude 기본 셋업 폴더

이 폴더 하나로 새 머신/새 계정에서 Claude Code 기본 환경을 재현한다.
구성: **① 글로벌 CLAUDE.md ② oh-my-claudecode(OMC) 플러그인 ③ mattpocock/skills ④ 로컬 스킬(repo-artifact-classify)**.

---

## Claude에게 지시할 때 (복붙용)

> `D:\claude-setup` 폴더를 읽고, README.md의 순서대로 Claude 환경을 세팅해줘.
> 이 폴더 밖의 다른 설정은 참고하지 말고, 각 단계마다 검증하고 결과를 보고해줘.

---

## 세팅 순서 (Claude가 수행)

### 1. 글로벌 CLAUDE.md 설치
이 폴더의 `global-CLAUDE.md`를 사용자 홈의 `~/.claude/CLAUDE.md`로 복사한다.
- Windows 경로: `C:\Users\<사용자>\.claude\CLAUDE.md`
- 이미 파일이 있으면 **덮어쓰기 전에 기존 내용을 보여주고 확인**을 받는다.
- 복사 후 파일이 존재하고 내용이 일치하는지 확인한다.

### 2. oh-my-claudecode(OMC) 설치
Claude Code 안에서 플러그인 마켓플레이스로 설치한다.
```
/plugin marketplace add Yeachan-Heo/oh-my-claudecode
/plugin install oh-my-claudecode@omc
```
- 설치 확인: `/plugin` 목록에 `oh-my-claudecode@omc`가 보이는지, `/autopilot`·`/team` 등 OMC 명령이 뜨는지 확인.
- 이미 설치돼 있으면 재설치 대신 최신 상태만 확인한다(`omc-setup` 스킬로 갱신 가능).
- 참고: OMC 에이전트 라우팅 규칙은 `~/.claude/rules/omc-agents.md`에 둔다(이 폴더의 `rules/` 참고).

### 3. mattpocock/skills 설치
터미널(셸)에서 실행한다. (Claude Code 슬래시 명령이 아니라 npx)
```
npx skills@latest add mattpocock/skills
```
- 설치 마법사에서 원하는 스킬과 코딩 에이전트(Claude Code)를 선택한다.
- **반드시 `setup-matt-pocock-skills` 스킬을 함께 선택**한다.
- 설치 후 Claude Code에서 `/setup-matt-pocock-skills`를 실행한다. 이 스킬이:
  - 이슈 트래커(GitHub / Linear / 로컬 파일) 선택을 묻고
  - triage 라벨 규칙을 묻고
  - 문서 저장 위치를 정해
  - `CLAUDE.md`(또는 `AGENTS.md`)에 Agent skills 블록을 써 준다.

### 4. 로컬 스킬 설치 (repo-artifact-classify)
이 폴더의 `skills/repo-artifact-classify/`를 사용자 홈으로 복사한다.
- 대상: `~/.claude/skills/repo-artifact-classify/SKILL.md`
  (Windows: `C:\Users\<사용자>\.claude\skills\repo-artifact-classify\SKILL.md`)
- 저장소 산출물 13분류 택소노미. "산출물 분류 / 이 파일 어디 / 저장소 정리" 요청 시 자동 트리거.
- 설치 확인: 새 세션에서 스킬 목록에 `repo-artifact-classify`가 뜨는지 확인.

### 5. 최종 검증
- `~/.claude/CLAUDE.md` 존재 및 내용 확인
- `/plugin` 목록에 OMC 확인
- `/help` 또는 스킬 목록에 mattpocock 스킬(`/tdd`, `/grill-me` 등) 확인
- 셋업 요약을 보고한다.

---

## 폴더 구성

| 파일/폴더 | 용도 |
|---|---|
| `README.md` | 이 셋업 지침(진입점) |
| `global-CLAUDE.md` | `~/.claude/CLAUDE.md`로 복사할 글로벌 원칙 |
| `rules/` | `~/.claude/rules/`로 복사할 글로벌 룰(frontend, omc-agents 등) |
| `skills/` | `~/.claude/skills/`로 복사할 로컬 스킬(repo-artifact-classify) |

---

## 출처 / 참고
- oh-my-claudecode: https://github.com/Yeachan-Heo/oh-my-claudecode
- mattpocock/skills: https://github.com/mattpocock/skills

## 주의
- CLAUDE.md·rules는 **강제 정책이 아니라 컨텍스트**다. 명령·파일 접근을 실제로 차단하려면 `settings.json`의 `permissions.deny`나 PreToolUse hook을 쓴다.
- 위 설치 명령은 각 저장소 기준이며, 버전에 따라 달라질 수 있으니 실행 전 최신 README를 확인한다.
