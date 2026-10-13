---
name: dev-workflow
description: GitLab 코드 저장소에서 개발 작업을 할 때 사용한다. 이슈 처리·기능 구현·버그 수정을 시작할 때, 브랜치 생성·커밋·push·MR 생성·코드 리뷰·머지·릴리스(태그)·버전·CHANGELOG 작업을 할 때 트리거. "이슈 #N 처리해", "이거 고쳐서 올려", "MR 올려", "머지해", "릴리스해", "v1.2.0 태그" 같은 요청. 사내 공통 브랜치·MR·AI 리뷰·SemVer 태그 릴리스·영어 로그 규칙과 순서를 담는다.
---

# 개발 워크플로우 (GitLab · Jenkins · JFrog)

사내 모든 코드 프로젝트 공통 규칙이다. 프로젝트별 정보(빌드 명령 등)는 그 저장소의 `CLAUDE.md`, 규칙 정본은 저장소의 `docs/WORKFLOW.md`(없으면 이 스킬)를 따른다.
코드 작성·push·MR·리뷰·머지는 에이전트가 하고, **사람은 작업 지시와 릴리스 태그 승인만 한다.**

## 적용 범위

- `git remote -v`가 사내 GitLab(`192.168.123.32` 또는 `gitlab.sol.anysell.duckdns.org`)을 가리키는 코드 저장소.
- 문서 볼트·개인 설정 저장소(예: claude-setup, Obsidian 볼트)에는 적용하지 않는다. 그 저장소의 규칙을 따른다.
- 저장소에 `develop`·`verify` 브랜치가 있고 아직 전환 전이면 그 저장소의 기존 방식을 따르고, 전환 여부는 사용자에게 묻는다.

## 작업 순서

### 1. 이슈
- 작업에 해당하는 이슈 번호를 확인한다(`glab issue list`, `glab issue view <N>`).
- 없으면 `glab issue create`로 만든다(템플릿 `.gitlab/issue_templates/` 사용).

### 2. 브랜치
- `main` 최신에서 분기: `git switch main && git pull && git switch -c <type>/<이슈번호>-<짧은설명>`
- 형식 `<type>/<이슈번호>-<짧은설명>`, type은 `feat`·`fix`·`change`·`remove`·`chore`. 예: `feat/12-login-page`
- `main`에서 직접 커밋하지 않는다.

### 3. 구현
- 로그 출력 문자열은 **영어 평문**(주석은 한국어 무방). 레벨: ERROR 즉시 조치 / WARN 잠재 문제 / INFO 정상 상태 변화 / DEBUG 개발용.
- 분산·디바이스 통신 코드는 메시지 앞에 추적 ID: `[Device-104] ...`, `[Request-9f3a] ...`
- **버전을 소스에 하드코딩하지 않는다.** Gradle `version = System.getenv('RELEASE_VERSION') ?: '0.0.0-SNAPSHOT'`, Python은 `pyproject.toml` placeholder(`0.0.0.dev0`)를 CI가 치환, 펌웨어는 `-DFW_VERSION`.
- 비밀값·토큰·`.env` 내용을 코드·테스트·로그에 넣지 않는다.

### 4. 로컬 검증 → push
- 프로젝트 `CLAUDE.md`의 빌드·테스트 명령을 실행하고 **그린일 때만** push: `git push -u origin HEAD`
- 커밋 메시지는 `<type>: <요약>` 형식. git 신원은 전역 설정 그대로 쓰고 `git config user.*`를 설정하지 않는다.

### 5. MR 생성
- `glab mr create --target-branch main --title "<type>: <요약>" --description "<템플릿 내용>" --remove-source-branch --yes`
- 제목 `<type>: <요약>` — CHANGELOG 초안이 이 제목을 읽는다.
- 설명은 `.gitlab/merge_request_templates/Default.md` 형식: 변경 요약, `Closes #<이슈번호>`, SemVer 영향, `## AI 리뷰`(다음 단계에서 채움).

### 6. AI 리뷰 — 반드시 별도 서브에이전트
- 코드를 작성한 이 세션이 스스로 리뷰하지 않는다. 리뷰용 서브에이전트를 띄운다(예: code-reviewer 계열 에이전트).
- 리뷰어에게는 **`git diff origin/main...HEAD`, 이슈 본문, 저장소의 `docs/AI-REVIEW.md`만** 넘긴다. 작성 의도나 변명을 같이 넘기지 않는다.
- `docs/AI-REVIEW.md`가 없으면 6개 관점으로 리뷰시킨다: 정확성 · 회귀 · 테스트 적정성 · 보안/비밀값 · 로깅 규칙 · 스펙 일치(범위 밖 변경·버전 하드코딩 없음).
- 판정이 FAIL이면 고치고 push한 뒤 **다시 리뷰**. PASS 또는 수정 후 PASS가 나올 때까지 머지하지 않는다.
- 결과를 MR 설명의 `## AI 리뷰` 절에 기록(`glab mr update <MR> --description ...`): 리뷰 에이전트·모델 / 판정 / 지적과 조치 / 실행한 검증 명령과 결과.

### 7. 머지
- MR에 파이프라인(Jenkins 커밋 상태)이 생겼는지 먼저 확인한다: `glab ci status` 또는 `glab mr view <MR>`. 아직 없으면 잠시 후 다시 확인한다 — 파이프라인이 생기기 전에 머지하면 CI 게이트를 건너뛸 수 있다.
- 저장소에 CI가 연결돼 있지 않으면(Webhook·Jenkins Job 없음) 파이프라인이 끝내 생기지 않고 auto-merge가 즉시 머지한다. 이때는 로컬 검증 결과에만 기대어 머지된 것이므로 완료 보고에 "CI 미연결 — 로컬 검증만"이라고 밝힌다.
- `glab mr merge <MR> --auto-merge --remove-source-branch --yes` — 파이프라인이 성공하면 GitLab이 머지한다. `--yes`가 없으면 확인 질문에서 멈춘다.
- `--auto-merge=false`로 즉시 머지하지 않는다. squash·rebase 옵션을 쓰지 않는다(CHANGELOG 초안이 머지 커밋을 읽는다).
- 머지 후 이슈는 `Closes #N`으로 자동으로 닫힌다. 손으로 닫지 않는다.

### 8. 릴리스 — 사용자가 요청할 때만
- 사용자가 "릴리스해", "vX.Y.Z 태그" 등으로 **명시적으로 요청한 경우에만** 진행한다. 머지했다고 스스로 릴리스하지 않는다.
- SemVer: Major = 하위 호환 깨짐 / Minor = 하위 호환 기능 추가 / Patch = 버그 수정만. 직전 태그 이후 MR들의 SemVer 영향 중 가장 큰 것으로 정하고, 사용자에게 제안한다.
- CHANGELOG: `bash scripts/changelog-draft.sh <직전 태그> <새 버전>` → `CHANGELOG.md`에 다듬어 넣고, 이것도 1~7 순서(브랜치 `chore/<N>-release-x.y.z` → MR → 리뷰 → 머지)로 반영한다.
- 고객·유관부서에 전달하는 릴리스면 `docs/RELEASE.md`도 비즈니스 언어로 작성.
- 태그: `git switch main && git pull && git tag -a vX.Y.Z -m "vX.Y.Z" && git push origin vX.Y.Z` — 이 명령은 확인 창이 뜬다(사람 승인 지점).
- 이후는 자동(Jenkins 태그 빌드 → JFrog `mgs-artifacts-local/<X.Y.Z>/<JOB>/` 업로드 → Rocket.Chat 알림). Jenkins 빌드 결과를 확인해 보고한다.

## 하지 않는 것

- `main`에 직접 push, force push, 보호 브랜치·태그 설정 변경
- 사용자 요청 없는 태그 생성·릴리스
- 리뷰 PASS 전 머지, 작성 세션의 자기 리뷰
- 비밀값을 프롬프트·MR·리뷰 기록·로그에 남기기

## 완료 보고

MR 링크, 리뷰 판정, 파이프라인 결과(CI 미연결이면 그 사실), 닫힌 이슈 번호를 짧게 보고한다. 릴리스면 태그·Jenkins 빌드 번호·JFrog 경로를 더한다.
