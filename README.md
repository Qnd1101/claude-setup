# claude-setup

Claude Code 개인 기본 환경을 **한 폴더로 재현**하는 셋업 번들.
새 머신/새 계정에서 이 저장소만 있으면 글로벌 원칙·플러그인·스킬을 그대로 복원한다.

---

## 무엇이 들어있나

| 경로 | 설치 위치 | 용도 |
|---|---|---|
| `global-CLAUDE.md` | `~/.claude/CLAUDE.md` | 범용 개인 작업 원칙(언어·최소 diff·git 안전·보안 등) |
| `rules/frontend.md` | `~/.claude/rules/frontend.md` | 프론트엔드/UI 작업 규칙 |
| `rules/omc-agents.md` | `~/.claude/rules/omc-agents.md` | OMC 에이전트 라우팅 규칙 |
| `skills/repo-artifact-classify/` | `~/.claude/skills/repo-artifact-classify/` | 저장소 산출물 13분류 스킬 |
| (외부) oh-my-claudecode | 플러그인 | 멀티에이전트 오케스트레이션 |
| (외부) mattpocock/skills | 스킬 | 엔지니어링 스킬 모음 |

---

## 빠른 시작

```bash
git clone https://github.com/Qnd1101/claude-setup.git
```

그다음 Claude Code에서:

> `claude-setup` 폴더를 읽고 README.md의 "세팅 순서"대로 환경을 세팅해줘.
> 이 폴더 밖의 설정은 참고하지 말고, 각 단계마다 검증하고 결과를 보고해줘.

---

## 거버넌스 (중요)

**글로벌 설정(`~/.claude/`의 CLAUDE.md·rules·skills)은 이 저장소를 단일 출처로 삼는다.**

- 글로벌 파일을 이 저장소 밖에서 직접 편집하지 않는다.
- 변경이 필요하면 → 이 저장소에서 수정 → 커밋/푸시 → 각 머신에 재배포.
- **예외 없음**: 애드혹으로 `~/.claude/CLAUDE.md`를 고치면 머신 간 드리프트 발생.
- 단, **프로젝트별** CLAUDE.md·`.claude/rules/`는 각 프로젝트 소관이며 이 규칙과 무관하다.

이 원칙은 `global-CLAUDE.md`의 `## 우선순위`에도 명시돼 있어 세션마다 로드된다.

---

## 세팅 순서 (Claude가 수행)

### 1. 글로벌 CLAUDE.md
`global-CLAUDE.md` → `~/.claude/CLAUDE.md`로 복사.
- Windows: `C:\Users\<사용자>\.claude\CLAUDE.md`
- 기존 파일이 있으면 **덮어쓰기 전 기존 내용을 보여주고 확인**받는다.
- 복사 후 존재·내용 일치 확인.

### 2. 글로벌 룰
`rules/*.md` → `~/.claude/rules/`로 복사.

### 3. oh-my-claudecode(OMC)
Claude Code 안에서:
```
/plugin marketplace add Yeachan-Heo/oh-my-claudecode
/plugin install oh-my-claudecode@omc
```
- 확인: `/plugin` 목록에 `oh-my-claudecode@omc`, `/autopilot`·`/team` 명령 노출.
- 이미 설치돼 있으면 `omc-setup` 스킬로 갱신만.
- OMC 설치 시 `~/.claude/CLAUDE-omc.md`가 자동 생성됨(이 저장소에 포함 안 함).

### 4. mattpocock/skills
터미널(셸)에서:
```
npx skills@latest add mattpocock/skills
```
- 마법사에서 원하는 스킬 + Claude Code 선택. **`setup-matt-pocock-skills` 반드시 선택**.
- 이후 Claude Code에서 `/setup-matt-pocock-skills` 실행(이슈 트래커·라벨·문서 위치 설정).

### 5. 로컬 스킬 (repo-artifact-classify)
`skills/repo-artifact-classify/` → `~/.claude/skills/repo-artifact-classify/`로 복사.
- 저장소 산출물 13분류 택소노미. "산출물 분류 / 이 파일 어디 / 저장소 정리" 요청 시 자동 트리거.
- 확인: 새 세션 스킬 목록에 `repo-artifact-classify` 노출.

### 6. 최종 검증
- `~/.claude/CLAUDE.md` 존재·내용
- `~/.claude/rules/` 파일들
- `/plugin` 목록에 OMC
- 스킬 목록에 mattpocock 스킬 + `repo-artifact-classify`
- 셋업 요약 보고.

---

## 출처
- oh-my-claudecode: https://github.com/Yeachan-Heo/oh-my-claudecode
- mattpocock/skills: https://github.com/mattpocock/skills

## 주의
- CLAUDE.md·rules는 **강제 정책이 아니라 컨텍스트**다. 명령·파일 접근을 실제로 차단하려면 `settings.json`의 `permissions.deny`나 PreToolUse hook을 쓴다.
- 외부 설치 명령은 각 저장소 버전에 따라 바뀔 수 있으니 실행 전 최신 README를 확인한다.
