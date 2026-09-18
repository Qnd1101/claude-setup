# 스킬 매니페스트

이 PC의 `~/.claude/skills/`에 실제로 설치된 스킬 전체 목록과 복원 명령. 새 머신에서는 이 파일 순서대로 실행한다.
갱신 기준일: 2026-09-17.

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

## 5. anthropics/skills (1개) — 프론트엔드 디자인

```bash
npx skills@latest add anthropics/skills --skill frontend-design -g -y
```

대시보드·홈페이지·랜딩 등 사용자 대면 UI를 만들 때 "generic AI 느낌"을 피하고 타이포·색·모션·레이아웃 방향성을 잡는다. 기존 자체 작성 `frontend-ui`를 대체(2026-09-15).

## 6. anthropics/knowledge-work-plugins (1개) — 기술 문서 작성

```bash
npx skills@latest add anthropics/knowledge-work-plugins --skill documentation -g -y
```

README·API 문서·런북·아키텍처 문서·온보딩 가이드의 유형별 필수 구성과 작성 원칙. "write docs / document this / 런북" 요청 시 트리거.

## 7. 이 저장소 `skills/` (10개) — 복사로 설치

`skills/<name>/` → `~/.claude/skills/<name>/` 복사.

| 스킬 | 출처 |
|---|---|
| `repo-artifact-classify` | 자체 작성 |
| `batch-grill-me` | mattpocock/skills 2026-07 설치본. 현재 upstream에서 삭제돼 이 저장소에 보관 |
| `design-an-interface` | 〃 |
| `edit-article` | 〃 |
| `obsidian-vault` | 〃 |
| `qa` | 〃 |
| `request-refactor-plan` | 〃 |
| `ubiquitous-language` | 〃 |
| `writing-great-skills` | 〃 |
| `humanizer-ko` | daleseo/korean-skills v1.6.0 보관본(이름 충돌 회피, 9절) |

## 8. UI 품질·접근성 (2개) — 2026-09-17 추가

```bash
npx skills@latest add vercel-labs/agent-skills --skill web-design-guidelines -g -y
npx skills@latest add addyosmani/web-quality-skills --skill accessibility -g -y
```

- `web-design-guidelines`(Vercel): 포커스·대비·터치 타깃·폼 등 웹 UI 체크리스트로 코드 리뷰.
- `accessibility`(Addy Osmani): WCAG 2.2 기준 접근성 감사·개선. 고령 사용자 대상 화면(큰 글씨·고대비·큰 버튼) 만들 때 사용.
- 설치 시 "PromptScript does not support global skill installation" 경고가 뜨지만 Claude Code 심링크는 정상 생성된다.

## 9. 글쓰기 — AI 티 제거 (2개) — 2026-09-17 추가

```bash
npx skills@latest add blader/humanizer -g -y
```

- `humanizer`(blader, 위키백과 「Signs of AI writing」 기반): not-X-but-Y 대비·한 줄 마무리·강제 3박자·대시 남발·굵은 라벨 같은 **구조** 패턴을 지운다. 사실은 바꾸지 않는다. 영어 기준이지만 구조 패턴은 한국어에도 그대로 통한다.
- `humanizer-ko`(daleseo/korean-skills, KatFishNet 논문 기반): **한국어 전용** 40패턴 — 쉼표 과다·연결어미 뒤 쉼표·번역투(~에 대해/~를 통해/되어진다)·「할 수 있다」 남발·한자어 서술어(진행하다/실시하다)·AI 유행어·단조로운 리듬. 보고서·절차서 문장을 다듬을 때 둘을 같이 쓴다.
- `humanizer-ko`는 upstream 폴더 이름이 `humanizer`라 blader와 **이름이 충돌**한다(`npx skills add`로 넣으면 서로 덮어쓴다). 그래서 이 저장소 `skills/humanizer-ko/`에 보관하고 복사로 설치한다(7절과 같은 방식). SKILL.md의 `name:`만 `humanizer-ko`로 바꿨고 나머지는 upstream v1.6.0 그대로.
- 용도: 업무일지·주간보고·결과보고·인수인계 문서에서 「AI로 쓴 티」 지적을 피한다. 수치·고유명사 보존을 스킬이 자체 점검하지만, 서버 값이 든 문서는 돌린 뒤 diff를 본다.

## 검증

새 세션의 스킬 목록에 위 57개가 모두 보이면 완료.

```bash
ls ~/.claude/skills | wc -l   # 57
```

## 갱신 절차

1. 이 PC에서 스킬을 추가·삭제했으면 이 파일의 해당 절을 고친다.
2. upstream에서 사라진 스킬은 `~/.agents/skills/<name>`을 `skills/`로 복사해 보관한다.
3. 커밋·푸시 후 다른 머신에서 재실행.
