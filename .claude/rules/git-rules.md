## Git 규칙

### 브랜치 전략
- `main`: 배포 가능한 상태만 유지, 직접 push 금지
- `develop`: 통합 브랜치 
- 작업 브랜치: `feature/이슈번호-간단설명`, `fix/이슈번호-간단설명`, `hotfix/설명`
- 브랜치는 항상 최신 `main`(또는 `develop`)에서 분기

### 커밋 메시지 (Conventional Commits)
- 형식: `type(scope): 설명`
- type: `feat`, `fix`, `refactor`, `test`, `docs`, `chore`, `style`, `perf`
- 예: `feat(user): 회원가입 이메일 인증 추가`
- 제목은 명령형, 50자 이내, 마침표 없음
- 본문이 필요하면 빈 줄 하나 띄우고 "무엇을"이 아니라 "왜"를 설명

### PR 규칙
- 제목은 커밋 컨벤션과 동일한 형식
- 작은 단위로 쪼개서 PR 생성 (한 PR = 하나의 논리적 변경)
- 머지 전 `./gradlew checkstyleMain`, `./gradlew test` 통과 필수
- 머지 방식: Squash and merge (커밋 히스토리 정리)

### 절대 하지 말 것
- `main`/`develop`에 강제 push (`--force`) 금지
- 시크릿/자격증명 포함된 커밋 금지 — 실수로 커밋 시 즉시 `git filter-repo`로 히스토리에서 제거 후 팀에 공지
- 의미 없는 커밋 메시지 (`fix`, `wip`, `수정`) 금지