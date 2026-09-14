# 스킬 매니페스트

이 PC의 `~/.claude/skills/`에 실제로 설치된 스킬 전체 목록과 복원 명령. 새 머신에서는 이 파일 순서대로 실행한다.
갱신 기준일: 2026-09-14.

## 설치 구조

- `npx skills add ... -g` 로 설치한 스킬은 `~/.agents/skills/<name>/`에 실제 파일이 놓이고 `~/.claude/skills/<name>`은 그 심링크다.
- 이 저장소 `skills/` 하위 스킬은 `~/.claude/skills/`로 직접 복사한다(심링크 아님).
- 설치 기록은 `~/.agents/.skill-lock.json`에 남는다. `npx skills update`가 이 파일을 읽어 갱신한다.

## 1. mattpocock/skills (33개)

```bash
npx skills@latest add mattpocock/skills -g -y \
  --skill ask-matt --skill claude-handoff --skill code-review --skill codebase-design \
  --skill diagnosing-bugs --skill domain-modeling --skill git-guardrails-claude-code \
  --skill grill-me --skill grill-with-docs --skill grilling --skill handoff --skill implement \
  --skill improve-codebase-architecture --skill loop-me --skill migrate-to-shoehorn \
  --skill prototype --skill research --skill resolving-merge-conflicts --skill scaffold-exercises \
  --skill setup-matt-pocock-skills --skill setup-pre-commit --skill setup-ts-deep-modules \
  --skill tdd --skill teach --skill to-questionnaire --skill to-spec --skill to-tickets \
  --skill triage --skill wayfinder --skill wizard --skill writing-beats --skill writing-fragments \
  --skill writing-shape
```

설치 후 Claude Code에서 `/setup-matt-pocock-skills` 실행.

## 2. vercel-labs/skills (1개)

```bash
npx skills@latest add vercel-labs/skills --skill find-skills -g -y
```

`/find-skills` — skills.sh 생태계에서 스킬 검색·설치.

## 3. bagelhole/DevOps-Security-Agent-Skills (6개) — 온프레미스 이중화·서버 운영

```bash
npx skills@latest add bagelhole/DevOps-Security-Agent-Skills -g -y \
  --skill linux-administration --skill systemd-services --skill backup-recovery \
  --skill mongodb --skill postgresql --skill load-balancing
```

| 스킬 | 용도 |
|---|---|
| `linux-administration` | 패키지·서비스·로그·방화벽. Debian/Ubuntu와 RHEL/CentOS(dnf·firewalld) 양쪽 절 있음 |
| `systemd-services` | 서비스 유닛·타이머·의존성·리소스 제한 |
| `backup-recovery` | rsync·Restic 백업 전략 |
| `mongodb` | 복제 구성(Replica Set)·keyfile 내부 인증·샤딩·백업 |
| `postgresql` | 스트리밍 복제·PITR·튜닝 |
| `load-balancing` | nginx·HAProxy 설정·헬스체크 |

## 4. github/awesome-copilot (1개) — RHEL 계열 장애 진단

```bash
npx skills@latest add github/awesome-copilot --skill centos-linux-triage -g -y
```

CentOS/Rocky/RHEL 기준으로 `systemctl`·`journalctl`·`dnf`·SELinux·firewalld를 전제로 진단·조치·검증·롤백 절차를 낸다.

## 5. 이 저장소 `skills/` (10개) — 복사로 설치

`skills/<name>/` → `~/.claude/skills/<name>/` 복사.

| 스킬 | 출처 |
|---|---|
| `frontend-ui` | 자체 작성 |
| `repo-artifact-classify` | 자체 작성 |
| `batch-grill-me` | mattpocock/skills 2026-07 설치본. 현재 upstream에서 삭제돼 이 저장소에 보관 |
| `design-an-interface` | 〃 |
| `edit-article` | 〃 |
| `obsidian-vault` | 〃 |
| `qa` | 〃 |
| `request-refactor-plan` | 〃 |
| `ubiquitous-language` | 〃 |
| `writing-great-skills` | 〃 |

## 검증

새 세션의 스킬 목록에 위 51개가 모두 보이면 완료.

```bash
ls ~/.claude/skills | wc -l   # 51
```

## 갱신 절차

1. 이 PC에서 스킬을 추가·삭제했으면 이 파일의 해당 절을 고친다.
2. upstream에서 사라진 스킬은 `~/.agents/skills/<name>`을 `skills/`로 복사해 보관한다.
3. 커밋·푸시 후 다른 머신에서 재실행.
