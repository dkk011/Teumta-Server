# 작업 규칙 (Git · PR)

1인 개발이지만 이슈 → 브랜치 → PR 순서를 지켜서 무엇을 왜 바꿨는지 기록을 남긴다.

## 1. 흐름

- `작업 브랜치 → dev → main`. **main에 직접 push하지 않는다.** main 반영도 PR로 한다.
- 모든 작업 브랜치는 `dev`에서 딴다. 포크하지 않고 같은 저장소에 브랜치를 만든다.
- 이슈를 먼저 만들고, 그 번호로 브랜치·커밋·PR을 묶는다. 이슈 템플릿은 Feature, Bug, Question.

## 2. 브랜치 이름

- `feat/<kebab-case>`, `fix/<kebab-case>` (예: `feat/station-stay-time`, `fix/midnight-last-train`)

## 3. 커밋

형식: `[#이슈번호]- 유형: 설명` · 유형 `feat` `fix` `refactor` `docs` `chore`

예: `[#12]- feat: 역별 머물 수 있는 시간 계산`

- 한 커밋에 한 가지 일. **빌드가 통과하는 지점**에서 끊는다.
- 이름 바꾸기와 값 바꾸기를 섞지 않는다. 이름만 바꾼 커밋에서 동작 변화가 없음을 먼저 확인한다.
- 의존성을 바꾸면 `build.gradle`(버전 카탈로그를 쓰면 `gradle/libs.versions.toml`까지) **같은 커밋에** 넣는다.
- 삭제는 "사용처 확인 → 삭제 → 빌드·테스트" 순서.

## 4. PR

- base는 `dev`. 본문에 `Closes #이슈번호`를 쓰고 PR 템플릿을 채운다.
- `Closes #`는 **기본 브랜치에 머지될 때만** 이슈를 자동으로 닫는다. 기본 브랜치를 `dev`로 두면 `dev` 머지로 닫힌다. 기본 브랜치가 `main`이면 직접 닫는다.
- PR은 작게. 사용처가 많은 변경(예: 공통 토큰·이름 치환)은 따로 뺀다.
- 본문에 **알려진 한계**를 적고 후속 이슈로 닫는다.
- 쌓아 올린 PR은 아래부터 순서대로 머지한다. 롤백은 `git revert`.
- 1인 개발이라 승인 리뷰는 없다. 대신 머지 전에 Files changed를 처음부터 끝까지 한 번 읽고, 고민한 부분은 셀프 리뷰 코멘트로 남긴다.

## 5. main 승격

- **1주일 단위, 스프린트가 끝날 때.** `dev`에 모은 기능을 `[Release] …` PR로 `main`에 올린다. 발표 전에는 반드시 한 번 승격한다.
- 본문에 포함된 PR 번호를 나열한다.
- 머지 전에 배포 환경변수를 먼저 갱신한다(EC2의 `.env`, GitHub Actions Secrets). 새 환경변수는 README 표에도 적는다.
- `main`이 `dev`보다 크게 뒤처지면 시연 환경이 최신이 아니게 되므로 주기를 지킨다.

### 웹 페이지 (`docs/`)

- `docs/`는 GitHub Pages(main 브랜치)로 공개된다. 개인정보처리방침·약관처럼 공개할 웹 페이지만 둔다.
- 페이지 수정도 `dev`를 거쳐 `[Release]` PR로 `main`에 올라가야 반영된다. 방침·약관을 바꾸면 시행일과 고지 기간(약관 7일, 불리한 변경 30일)을 같이 확인한다.

## 6. PR 검사 (CI)

- `dev`·`main` 대상 PR에서 `./gradlew build`(컴파일 + 단위 테스트)를 자동 실행한다.
- DB가 필요한 통합 테스트는 나중에 추가한다.
- 워크플로(`.github/workflows/`)는 Gradle 프로젝트를 만든 뒤 추가한다.

## 7. 저장소 설정 (첫날)

- 기본 브랜치: `dev`
- `dev`·`main` 보호 규칙: PR 필수, 검사 통과 필수(Require status checks to pass), 강제 push·삭제 금지. 1인 개발이라 필수 승인 수는 0.
- 비밀값은 저장소에 올리지 않는다. 로컬은 `.env`·`application-local.yml`, 배포는 EC2의 `.env`와 GitHub Actions Secrets에 둔다.
- 포맷터: 첫 주에 정한다(후보: Spotless + google-java-format).
