## 코드 스타일 메모 

---
paths:
- "**/*.java"
---

## 코드 스타일
- 베이스: 구글 Java 스타일 가이드 (checkstyle: `config/google_checks.xml`)
- 예외: 줄 길이는 120자로 확장 (네이버 컨벤션 참고)
- IntelliJ 포매터: `.idea/codeStyles/Project.xml`에 위 설정 반영, 수동 포맷 조정 금지
- PR 전 `./gradlew checkstyleMain` 필수 통과
- 
### 네이밍
- 클래스: PascalCase, 인터페이스에 `I` 접두사 금지 (`UserService`, `UserServiceImpl`)
- 테스트 메서드: `should_기대결과_when_조건()` 형식
- Boolean 필드/메서드: `is`, `has` 접두사 (`isActive`, `hasPermission`)

### 로깅
- `System.out.println` 금지, 반드시 SLF4J `log.info/debug/error` 사용
- 예외 로깅 시 스택트레이스 포함 (`log.error("msg", e)`)

### Optional / Null 처리
- 반환 타입에 `Optional<T>` 쓰되 필드/파라미터에는 사용 금지
- `null` 반환 대신 `Optional.empty()` 또는 예외

### 주석
- 비즈니스 로직상 "왜"를 설명하는 주석만 허용 ("무엇"은 코드로 드러나야 함)

### Import
- Wildcard import(`import java.util.*`) 금지 — Checkstyle로 강제 가능하면 여기서 제외

