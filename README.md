# Common Claude - 공통 Claude 설정 템플릿

이 프로젝트는 Claude Code 설정을 재사용하기 위한 공통 템플릿입니다.

새로운 Java/Spring Boot 프로젝트를 시작할 때 이 템플릿을 기반으로 복제하면, Claude Code 설정을 바로 적용할 수 있습니다. 

## 주요 목적

1. **Claude Code 설정 공유** - `.claude/` 폴더의 설정을 모든 프로젝트에서 재사용
2. **표준화된 구조** - Spring Boot 프로젝트의 일관된 폴더 구조와 패턴 제공
3. **개발 규칙 통일** - 코드 스타일, Git 워크플로우, 커밋 규칙을 하나의 템플릿으로 관리

## 이 템플릿이 제공하는 것

### Claude Code 설정 (`.claude/` 폴더에 완성된 상태로 제공)

- **CLAUDE.md** — Claude가 프로젝트의 구조와 개발 방식을 이해하도록 하는 문서. 기술 스택, 아키텍처, 개발 패턴 등을 정리합니다.
- **setting.json** — Claude Code의 권한 설정(어떤 명령은 허용하고 어떤 명령은 차단할지)과 언어 설정.
- **rules/** — 코드 스타일, Git 워크플로우, 커밋 규칙 등 팀이 따를 개발 규칙들.

### 잘 설계된 Spring Boot 프로젝트 구조

- **Domain-Driven Design** — 비즈니스 도메인(회원, 상품)별로 코드가 정리됩니다.
- **중앙화된 예외 처리** — 모든 예외가 한 곳에서 관리되어 API 에러 응답이 일관되고 클라이언트 개발이 쉬워집니다.
- **JPA + 리포지토리 패턴** — Spring Data JPA를 통해 데이터 접근 계층이 간단하게 구성되고 복잡한 쿼리도 타입 안전하게 작성할 수 있습니다.
- **개발/테스트 인프라 준비 완료** — H2 인메모리 데이터베이스와 JUnit 5로 즉시 테스트를 시작할 수 있습니다.

## 기술 스택

| 기술 | 버전 |
|------|------|
| **Java** | 17 |
| **Spring Boot** | 4.1.0 |
| **Gradle** | 8.x (Kotlin DSL) |
| **JPA/Hibernate** | Spring Data JPA |
| **Database** | H2 (개발/테스트) |
| **Logging** | SLF4J + Log4J |
| **Code Generation** | Lombok |
| **Testing** | JUnit 5 |

## 프로젝트 구조

```
common-cluade/
├── .claude/                              # Claude Code 설정 (중요!)
│   ├── CLAUDE.md                         # Claude와의 협업 가이드
│   ├── setting.json                      # Claude Code 권한 및 언어 설정
│   └── rules/                            # 개발 규칙
│       ├── code-style.md                 # 코드 스타일 가이드
│       └── git-rules.md                  # Git 워크플로우 규칙
├── src/main/java/common/claude/commoncluade/
│   ├── CommonCluadeApplication.java      # Spring Boot 진입점
├── src/main/resources/
│   └── application.properties            # Spring 설정
├── src/test/java/common/claude/commoncluade/
│   └── 테스트 클래스들
├── gradle/                               # Gradle 설정
├── build.gradle.kts                      # Gradle 빌드 파일
├── settings.gradle.kts                   # Gradle 프로젝트 설정
└── README.md                             # 이 파일
```

**중요: `.claude/` 폴더**
- 새 프로젝트를 만들 때 이 폴더를 통째로 복사하면 설정이 자동으로 적용됩니다
- 프로젝트별 특화된 내용은 여기서만 수정하면 됩니다


### `.claude/setting.json` — Claude의 "행동 규칙"

Claude Code가 어떤 명령을 실행할 수 있는지, 어떤 파일은 건드려야 하는지를 정의합니다. 이를 통해 안전하면서도 생산적인 협업이 가능해집니다:

```json
{
  "$schema": "https://json-schema.org/claude-code-settings.json",
  "language": "korean",
  "permissions": {
    "allow": [
      "Bash(./gradlew build:*)",
      "Bash(./gradlew test:*)",
      "Bash(./gradlew bootRun:*)",
      "Bash(./gradlew clean:*)",
      "Bash(./gradlew checkstyleMain:*)",
      "Bash(git status)",
      "Bash(git diff:*)",
      "Bash(git log:*)",
      "Bash(git branch:*)",
      "Edit(*)",
      "Read(*)",
      "Write(*)"
    ],
    "deny": [
      "Bash(rm -rf:*)",
      "Bash(git push --force:*)",
      "Bash(git reset --hard:*)",
      "Bash(curl:*)"
    ]
  }
}
```

**각 설정이 의미하는 것:**

- **language: korean** — Claude가 한국어로 소통하며, 코드 주석과 문서도 한국어로 작성합니다.
- **allow** — 허용되는 작업들. Gradle 빌드/테스트, Git 상태 확인, 파일 편집이 자유롭게 가능합니다. 이를 통해 Claude가 코드를 실행해보고 테스트하며 개선할 수 있습니다.
- **deny** — 금지되는 작업들. 위험한 명령(전체 삭제, 강제 푸시, 히스토리 변경)이 실수로 실행되지 않도록 막습니다.

### `.claude/CLAUDE.md` — Claude의 "프로젝트 설명서"

Claude가 당신의 프로젝트를 완벽하게 이해하도록 도와주는 문서입니다:

- **프로젝트 개요** — 이 프로젝트가 어떤 목적인지, 어떤 기능을 구현하는지
- **기술 스택** — 사용하는 프레임워크, 라이브러리, 버전들
- **빌드 및 테스트 명령어** — Claude가 코드를 검증하기 위해 실행해야 할 명령어들
- **프로젝트 구조** — 각 폴더가 무엇을 담당하는지, 어디에 새 코드를 추가해야 하는지
- **아키텍처 패턴** — 도메인 구조, 에러 처리 방식, 데이터베이스 접근 방식 등
- **개발 설정** — IDE 설정, 데이터베이스 연결 방식 등 개발환경 관련 정보

### `.claude/rules/` — 팀의 "개발 규칙"

일관되고 안정적인 코드를 유지하기 위한 규칙들입니다:

- **code-style.md** — Google Java Style Guide를 기반으로 한 코드 스타일. 줄 길이, 네이밍 규칙, 주석 작성법 등을 명확히 정의합니다. Claude는 이 규칙을 따라 코드를 작성합니다.
- **git-rules.md** — Git 브랜치 전략(main, develop, feature), 커밋 메시지 형식(feat, fix, refactor 등) 등. 일관된 Git 히스토리를 유지하게 합니다.

### 새 프로젝트를 만들 때

`.claude/` 폴더를 재사용하면 매번 처음부터 설정할 필요가 없습니다:

1. **이 템플릿 프로젝트 복제** — `common-cluade`를 새 프로젝트로 복제
2. **`.claude/` 폴더 그대로 유지** — 설정이 이미 완성된 상태
3. **필요시 프로젝트별 맞춤** — 프로젝트 특성에 맞게 `CLAUDE.md`의 기술 스택, 구조 등만 수정

## MCP 서버로 최신 문서에 접근하기

이 프로젝트는 **Model Context Protocol (MCP)** 서버를 통해 외부 도구와 데이터를 Claude에 연결합니다. 덕분에 Claude가 항상 최신의, 정확한 정보를 기반으로 조언해줄 수 있습니다.

### `.mcp.json` 설정

```json
{
  "mcpServers": {
    "context7": {
      "type": "http",
      "url": "https://mcp.context7.com/mcp"
    }
  }
}
```

### context7이란? — "최신 공식 문서 검색 도구"

**context7**은 React, Spring Boot, Gradle, Spring Data JPA 등 주요 라이브러리와 프레임워크의 **최신 공식 문서**를 Claude에게 실시간으로 제공합니다. 

당신이 기술적인 질문을 하면, Claude는 단순히 훈련 데이터에 의존하는 것이 아니라 context7을 통해 현재 시점의 정확한 공식 문서를 검색하고 그에 기반해 답변합니다. API가 변경되었거나 새로운 기능이 추가된 경우에도 즉시 최신 정보를 반영한 답변을 얻을 수 있습니다.

#### 실제 사용 예시

Claude와의 대화에서 아래와 같은 질문을 하면 context7이 자동으로 최신 공식 문서를 조회합니다:

**Spring Boot 설정 관련**
- "Spring Boot에서 H2 데이터베이스 설정하는 정확한 방법은?"
- "Spring Boot 최신 버전에서 JPA 자동 DDL 설정이 어떻게 바뀌었어?"

**Gradle 설정**
- "build.gradle.kts에서 Spring Data JPA 의존성을 추가하는 정확한 방식은?"
- "Lombok을 Gradle Kotlin DSL로 설정할 때 주의할 점이 뭐야?"

**Spring Data JPA 심화**
- "JpaRepository에서 복잡한 커스텀 쿼리를 타입안전하게 만드는 방법은?"
- "Spring Data JPA에서 페이징과 정렬을 함께 사용할 때 성능 최적화 방법은?"

**API 세부사항**
- "JPA @Entity 어노테이션의 모든 속성과 그 의미는?"
- "@Transactional의 propagation, isolation 옵션들이 실제로 어떻게 동작해?"

#### 동작 원리 — 자동으로 일어나는 일들

당신이 위의 질문들을 Claude에게 하면, 백그라운드에서 이런 일들이 자동으로 일어납니다:

1. **Claude가 문제를 인식** — 당신의 질문이 특정 라이브러리/프레임워크에 대한 것임을 파악
2. **context7 MCP 서버 호출** — 해당 라이브러리의 최신 공식 문서 검색 요청
3. **최신 정보 검색** — context7이 공식 문서 사이트에서 현재 버전의 정확한 정보 조회
4. **정확한 답변 생성** — 검색한 최신 문서를 기반으로 당신의 질문에 정확히 답변

이 덕분에 당신은 **마치 공식 문서를 직접 읽으면서 Claude의 설명을 동시에 듣는 경험**을 하게 됩니다.

## 빠른 시작

### 1단계: 이 템플릿으로 새 프로젝트 만들기

```bash
# 이 템플릿 프로젝트를 새로운 프로젝트로 복제
git clone <repository-url> my-new-project
cd my-new-project

# 프로젝트명, 패키지명 등을 자신의 프로젝트에 맞게 수정
# 중요: .claude/ 폴더는 그대로 유지 (다른 프로젝트에도 재사용 가능)
```

### 2단계: 프로젝트 환경 준비

```bash
# 모든 의존성을 다운로드하고 프로젝트를 빌드합니다.
./gradlew build

# 빌드가 완료되면 IDE를 열어 추가 설정을 진행합니다.
```

### 3단계: IDE 설정 (IntelliJ IDEA)

Lombok이 정상작동하려면 IDE에서 어노테이션 처리를 활성화해야 합니다:

1. **IntelliJ IDEA 메뉴** → **Settings** 열기
2. **Build, Execution, Deployment** → **Compiler** → **Annotation Processors**
3. ☑ **Enable annotation processing** 체크
4. **Apply** 후 **OK** 클릭

설정이 완료되면 Gradle이 자동으로 프로젝트를 동기화합니다.
## 개발하면서 자주 쓰는 명령어들

### 프로젝트 빌드 및 실행

```bash
# 전체 프로젝트 빌드 (테스트 포함)
./gradlew build

# 테스트 스킵하고 빌드만 (개발 중 빠른 피드백 필요할 때)
./gradlew build -x test

# 애플리케이션 실행
./gradlew bootRun

# 코드 스타일 검증 (PR 머지 전에 반드시 확인)
./gradlew checkstyleMain

# 클린 빌드 (의존성 문제가 의심될 때)
./gradlew clean build
```

### 테스트 실행

```bash
# 전체 테스트 실행
./gradlew test

# 특정 테스트 클래스만 실행
./gradlew test --tests ClassName

# 특정 테스트 메소드만 실행
./gradlew test --tests ClassName.methodName
```

### 데이터베이스 접근

프로젝트는 개발 편의성을 위해 **H2 인메모리 데이터베이스**를 사용합니다. 실제 데이터를 시각적으로 확인해야 할 때:

```bash
# 1. 먼저 애플리케이션을 실행합니다.
./gradlew bootRun

# 2. 브라우저에서 아래 주소로 접속합니다.
# http://localhost:8080/h2-console

# 3. 다음 정보로 로그인합니다.
# JDBC URL: jdbc:h2:mem:testdb
# User Name: sa
# Password: (빈 값)
```

H2 콘솔에서 직접 SQL을 실행하거나 테이블 구조를 확인할 수 있습니다.

## 아키텍처의 주요 특징들

### BaseEntity — 모든 엔티티의 기반

모든 도메인 엔티티(Member, Product 등)는 `BaseEntity`를 상속합니다. 덕분에 모든 엔티티가 자동으로 기본적인 필드들을 갖습니다:

```java
@Entity
public class Product extends BaseEntity {
    // 자동으로 제공되는 필드들:
    // - id (Long): 자동 생성되는 기본 키
    // - createdAt (LocalDateTime): 생성 시각 (자동 기록)
    // - updatedAt (LocalDateTime): 마지막 수정 시각 (자동 기록)
    
    // 도메인 특화 필드들만 여기서 추가하면 됩니다
    private String name;
    private BigDecimal price;
}
```

이렇게 하면 새 엔티티를 만들 때마다 `id`, 생성일시, 수정일시 같은 반복 코드를 작성할 필요가 없고, 모든 엔티티가 동일한 감사(audit) 정보를 일관되게 유지합니다.


## 코드 스타일 — 일관되고 읽기 쉬운 코드 작성

이 프로젝트의 모든 코드는 **Google Java Style Guide**를 따릅니다. 덕분에 누가 코드를 봐도 친숙하고, Claude의 제안도 이 표준에 맞춰져 있습니다.

**주요 규칙들:**
- **줄 길이**: 최대 120자 (Google 기본값 100자보다 약간 확장해서 가독성과 실용성의 균형)
- **네이밍**: 클래스는 PascalCase(`UserService`), 메서드와 변수는 camelCase(`getUserName`)
- **Boolean 필드**: `is`나 `has` 접두사 사용 (`isActive`, `hasPermission`)
- **테스트 메서드**: `should_기대결과_when_조건` 형식으로 명확하게 (`should_return_empty_list_when_no_members_exist`)
- **로깅**: `System.out.println` 절대 금지, 반드시 SLF4J 사용 (`log.info("message")`)

**자동 검증:**

```bash
# PR을 머지하기 전에 반드시 이 명령어로 코드 스타일 검증
./gradlew checkstyleMain
```

자세한 내용은 [.claude/rules/code-style.md](.claude/rules/code-style.md)를 참고하세요.

## Git 워크플로우 — 안정적이고 추적 가능한 개발

이 프로젝트는 체계적인 Git 전략을 따라 개발 히스토리가 명확하고 배포가 안전합니다.

### 브랜치 전략

- **main** — 항상 배포 가능한 상태. 직접 push는 금지, PR을 통해서만 머지 가능
- **develop** — 여러 기능이 통합되는 브랜치. 테스트 완료된 기능들이 여기로 머지됨
- **feature/** — 새 기능을 개발할 때 (`feature/이슈번호-기능설명` 형식)
  - 예: `feature/auth-001-email-verification`
- **fix/** — 버그를 수정할 때 (`fix/이슈번호-버그설명` 형식)
  - 예: `fix/bug-042-null-pointer-validation`

### 커밋 메시지 규칙 (Conventional Commits)

커밋 메시지의 첫 줄은 다음 형식을 따릅니다. 이렇게 하면 나중에 변경 이력을 한 눈에 파악할 수 있습니다:

```
feat(scope): 짧은 설명
fix(scope): 짧은 설명
refactor(scope): 짧은 설명
test(scope): 테스트 추가/수정
docs: 문서 업데이트
chore: 빌드, 의존성, 설정 변경
```

**예시들:**
- `feat(member): 회원가입 기능 추가`
- `fix(product): 상품 조회 버그 해결`
- `refactor(product): ProductRepository 쿼리 최적화`
- `test(member): 회원 생성 시나리오 테스트 추가`
- `docs: README.md 설정 가이드 업데이트`

**중요한 약속:**
- 50자 이내의 짧은 제목 (제목에 마침표 없음)
- 제목은 "명령형"으로 작성 ("추가했다" ❌, "추가" ✅)
- 세부 설명이 필요하면 제목 다음에 빈 줄을 한 개 띄우고 작성

### PR 규칙

- PR은 하나의 논리적 변경만 포함 (작고 리뷰하기 쉬운 사이즈)
- 머지 전에 `./gradlew checkstyleMain`과 `./gradlew test` 반드시 통과
- 머지 방식: **Squash and merge** (중간 커밋들을 정리하고 깔끔한 히스토리 유지)

자세한 내용은 [.claude/rules/git-rules.md](.claude/rules/git-rules.md)를 참고하세요.


## 참고 문서

- [CLAUDE.md](.claude/CLAUDE.md) - 프로젝트 상세 가이드
- [Git Rules](.claude/rules/git-rules.md) - Git 워크플로우 규칙
- [Code Style](.claude/rules/code-style.md) - 코드 스타일 가이드
- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [Spring Data JPA Documentation](https://spring.io/projects/spring-data-jpa)

## 라이선스

MIT License

