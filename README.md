# Common Claude - 공통 Claude 설정 템플릿

이 프로젝트는 **Claude Code와의 일관된 개발 경험을 제공하는 공통 설정 템플릿**입니다.

새로운 Java/Spring Boot 프로젝트를 시작할 때 이 프로젝트 구조를 기반으로 복제하여 사용하면, 팀 전체가 동일한 Claude 설정, 코드 스타일, Git 규칙을 적용할 수 있습니다.

## 주요 목적

1. **Claude Code 설정 공유** - `.claude/` 폴더의 설정을 모든 프로젝트에서 재사용
2. **표준화된 구조** - Spring Boot 프로젝트의 일관된 폴더 구조와 패턴 제공
3. **개발 규칙 통일** - 코드 스타일, Git 워크플로우, 커밋 규칙을 하나의 템플릿으로 관리

## 제공되는 것

### Claude Code 설정 (`.claude/` 폴더)
- **CLAUDE.md** - Claude Code와 협업하기 위한 프로젝트 가이드
- **setting.json** - Claude Code 권한 설정 및 언어 설정
- **rules/** - 코드 스타일, Git 규칙 등 개발 규칙

### 프로젝트 구조
- 표준화된 Spring Boot 프로젝트 구조
- Domain-Driven Design 패턴
- 중앙화된 예외 처리 및 에러 코드 관리
- JPA + 리포지토리 패턴 설정
- 개발/테스트 인프라 (H2, JUnit 5)

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
| **AI Integration** | Spring AI 2.0.0 |
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

## Claude Code 설정 (.claude/)

이 템플릿은 Claude Code와의 협업을 위한 완전한 설정을 제공합니다.

### `.claude/setting.json`

Claude Code의 동작을 제어하는 설정 파일입니다:

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

**설정 내용:**
- **language**: `korean` - Claude와의 소통을 한국어로 진행
- **allow**: Gradle 명령어, Git 명령어, 파일 수정 허용
- **deny**: 위험한 삭제, 강제 푸시, 히스토리 변경, 외부 통신 차단

### `.claude/CLAUDE.md`

이 프로젝트에 대한 Claude의 이해도를 높이는 문서:
- 프로젝트 개요 및 기술 스택
- 빌드 및 테스트 명령어
- 프로젝트 구조 및 아키텍처 패턴
- 개발 설정 및 일반적인 작업 가이드

### `.claude/rules/`

팀의 개발 규칙을 정의합니다:
- **code-style.md** - Google Java Style Guide 기반 코드 스타일
- **git-rules.md** - Git 워크플로우 및 커밋 메시지 규칙

**새 프로젝트를 만들 때:**
1. 이 템플릿 프로젝트를 복제
2. `.claude/` 폴더를 새 프로젝트에 복사
3. 필요시 `CLAUDE.md`와 `rules/` 문서를 프로젝트에 맞게 수정

## 빠른 시작

### 1. 이 템플릿 프로젝트 사용

```bash
# 템플릿 프로젝트 클론
git clone <repository-url> my-new-project
cd my-new-project

# .claude/ 폴더는 그대로 유지 (다른 프로젝트에도 복사 가능)
# 프로젝트명, 패키지명 등을 필요에 맞게 수정
```

### 2. 프로젝트 설정

```bash
# 의존성 다운로드 및 빌드
./gradlew build

# IDE 설정 (IntelliJ IDEA)
# Settings → Build, Execution, Deployment → Compiler → Annotation Processors
# → Enable annotation processing 활성화 (Lombok 처리용)
```
## 개발 환경 설정

### IDE 설정 (IntelliJ IDEA)

1. 프로젝트를 IntelliJ IDEA에서 열기
2. **Settings** → **Build, Execution, Deployment** → **Compiler** → **Annotation Processors**
   - ☑ Enable annotation processing 활성화 (Lombok 처리용)
3. Gradle이 자동으로 동기화됨

### 데이터베이스

- **H2 콘솔**: 애플리케이션 실행 중 `http://localhost:8080/h2-console` 접속
- **연결 문자열**: `jdbc:h2:mem:testdb`
- **사용자명**: `sa`
- **비밀번호**: (빈 값)

## 주요 특징

### BaseEntity (기본 엔티티)

모든 도메인 엔티티는 `BaseEntity`를 상속합니다:
```java
@Entity
public class Product extends BaseEntity {
    // 자동으로 제공: id, createdAt, updatedAt 등
}
```


## 코드 스타일

- **기준**: Google Java Style Guide
- **줄 길이**: 120자 (기본값보다 확장)
- **Checkstyle 검증**: PR 머지 전 필수

```bash
./gradlew checkstyleMain
```

자세한 내용은 [.claude/rules/code-style.md](.claude/rules/code-style.md) 참고

## Git 워크플로우

- **main**: 배포 가능한 상태 (직접 push 금지)
- **develop**: 통합 브랜치
- **feature/**: 기능 개발 (`feature/이슈번호-설명`)
- **fix/**: 버그 수정 (`fix/이슈번호-설명`)

### 커밋 메시지 규칙 (Conventional Commits)

```
feat(scope): 짧은 설명
fix(scope): 짧은 설명
refactor(scope): 짧은 설명
test(scope): 짧은 설명
docs: 문서 업데이트
chore: 빌드, 설정 변경
```

예: `feat(member): 회원가입 이메일 인증 추가`

자세한 내용은 [.claude/rules/git-rules.md](.claude/rules/git-rules.md) 참고


## 참고 문서

- [CLAUDE.md](.claude/CLAUDE.md) - 프로젝트 상세 가이드
- [Git Rules](.claude/rules/git-rules.md) - Git 워크플로우 규칙
- [Code Style](.claude/rules/code-style.md) - 코드 스타일 가이드
- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [Spring Data JPA Documentation](https://spring.io/projects/spring-data-jpa)

## 라이선스

MIT License

