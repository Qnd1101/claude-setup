# OMC Agent Routing Rules

## Baseline

- Use OMC built-in agents and native OMC modes.
- Do not manually recreate agents.
- OMC controls actual model routing and agent execution.
- Model labels such as haiku, sonnet, and opus are guidance only.
- Prefer psmux-native workflow.
- Do not use tmux-based `/omc-teams` by default.
- Use `/autopilot`, `ulw`, `ultrawork`, `/team`, or `/oh-my-claudecode:team` for OMC execution.
- If a listed agent or command is unavailable in the installed OMC version, report it and use the closest supported OMC command or keyword.

## Build & Analysis Agents

| Agent | Model Guidance | Role | Use When |
|---|---|---|---|
| explore | haiku | Code exploration, file mapping, symbol mapping | Need to understand project structure or locate relevant files |
| analyst | opus | Requirement analysis, acceptance criteria, edge cases | Feature is unclear or needs acceptance criteria |
| planner | opus | Execution order, migration sequence, task breakdown | Need a safe implementation plan before edits |
| architect | opus | System design, API/interface design, architecture decisions | Structure or interface design is needed |
| debugger | sonnet | Root cause analysis, regression isolation | Error, stack trace, runtime bug, unexpected behavior |
| executor | sonnet | Code implementation and refactoring | Normal implementation or multi-file edits |
| deep-executor | opus | Complex autonomous implementation | Large, complex, multi-file implementation |
| verifier | sonnet | Evidence collection and completion verification | Need to confirm the work is truly complete |

## Review Agents

| Agent | Model Guidance | Role | Use When |
|---|---|---|---|
| quality-reviewer | sonnet | Logic defects, anti-patterns, performance issues | General code quality check |
| security-reviewer | sonnet | Security issues, auth/authz, SQL injection risks | Before deployment or security-sensitive changes |
| code-reviewer | opus | Comprehensive review, API contracts, compatibility | Final PR-level review |

Direct review triggers:
- `review code`
- `security review`

## Domain Specialist Agents

| Agent | Model Guidance | Role | Use When |
|---|---|---|---|
| test-engineer | sonnet | Test strategy, coverage, TDD | Need unit/integration/E2E test design or implementation |
| build-fixer | sonnet | Build, type, lint error repair | Build fails, type errors, lint errors |
| designer | sonnet | UI/UX design and component design | UI layout, UX flow, visual quality, component improvement |
| writer | haiku | Technical docs, README, comments | Documentation or explanatory writing |
| qa-tester | sonnet | Runtime verification, E2E testing | Need to run and verify behavior |
| scientist | sonnet | Data/statistical analysis | Logs, CSV, metrics, patterns, data analysis |
| document-specialist | sonnet | External official docs lookup | Need official framework/library documentation |
| git-master | sonnet | Git, commits, branch strategy, PR text | Commit cleanup, PR description, branch flow |
| code-simplifier | opus | Simplification, duplicate removal, cleanup | Complex code needs simplification |
| critic | opus | Critical review of plans/designs | Need to find weaknesses in a plan or architecture |

## Recommended Routing

### Simple bug or question

Use normal conversation first.
Escalate to `debugger` only if root cause analysis is needed.

### Find related files

Use:

`explore 에이전트: 이 기능과 관련된 파일과 심볼을 찾아줘.`

### Requirements and acceptance criteria

Use:

`analyst 에이전트: 이 기능의 요구사항, 엣지케이스, 수용 기준을 정리해줘.`

### Plan before risky edits

Use:

`planner 에이전트: 이 변경의 작업 순서와 위험 요소를 정리해줘.`

### Architecture or API design

Use:

`architect 에이전트: 확장 가능한 구조와 인터페이스를 설계해줘.`

Then optionally:

`critic 에이전트: 이 설계의 약점과 빠진 조건을 찾아줘.`

### Implementation

Use:

`executor 에이전트: 계획에 따라 안전하게 구현해줘.`

For large multi-file implementation:

`ulw: 여러 파일에 걸친 변경을 병렬로 안전하게 구현해줘.`

### Complex implementation

Use:

`deep-executor 에이전트: 대규모 다파일 구현을 진행해줘. 변경 전 계획과 검증 기준을 먼저 제시해줘.`

### Verify completion

Use:

`verifier 에이전트: 요구사항이 모두 충족됐는지 증거 기반으로 확인해줘.`

### Code quality review

Use:

`review code`

### Security review

Use:

`security review`

### Build or type errors

Use:

`fix build`

or:

`build-fixer 에이전트: 빌드/타입/린트 에러를 고쳐줘.`

### UI/UX work

Prefer project skills first:

`/ui-work`

`/ui-polish`

When using OMC agent routing:

`designer 에이전트: 현재 UI/UX 문제와 개선안을 정리해줘.`

### Test work

Use:

`tdd: 기능명`

or:

`test-engineer 에이전트: 테스트 전략과 테스트 코드를 작성해줘.`

### Runtime QA

Use:

`qa-tester 에이전트: 실제 실행 기준으로 기능을 검증해줘.`

### Documentation

Use:

`writer 에이전트: 변경사항과 사용법을 문서화해줘.`

### External official docs

Use:

`document-specialist 에이전트: 관련 공식 문서를 확인하고 현재 프로젝트에 맞게 요약해줘.`

### Data analysis

Use:

`scientist 에이전트: 이 로그/CSV/메트릭 데이터를 분석하고 패턴을 정리해줘.`

### Git work

Use:

`git-master 에이전트: 변경 파일을 의미 있는 단위로 정리하고 커밋/PR 설명을 작성해줘.`

### Simplification

Use:

`code-simplifier 에이전트: 이 복잡한 로직을 단순화하고 중복을 제거해줘.`

### Critical review

Use:

`critic 에이전트: 이 계획의 약점, 리스크, 빠진 조건을 찾아줘.`
