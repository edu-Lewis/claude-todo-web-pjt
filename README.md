# 📝 Todo List

카테고리별로 할 일을 관리할 수 있는 간단한 웹 기반 Todo List 앱입니다. 별도의 빌드 과정이나 프레임워크 없이 순수 HTML/CSS/JavaScript로 작성되었습니다.

## 주요 기능

- **할 일 추가**: 텍스트와 카테고리(개인/공부/업무/취미)를 지정해 할 일을 추가
- **할 일 수정**: 인라인 편집으로 텍스트와 카테고리 변경
- **완료 처리**: 체크 버튼으로 완료 상태 토글
- **삭제**: 확인 후 할 일 삭제
- **카테고리 필터**: 탭을 눌러 전체/개인/공부/업무/취미별로 목록 확인
- **진행률 표시**: 전체 진행률과 카테고리별 진행률을 프로그레스 바로 표시
- **회원가입 / 로그인**: Supabase Auth(이메일/비밀번호) 기반. 로그인하지 않으면 할 일 목록 대신 로그인/회원가입 화면이 표시됨
- **데이터 저장**: Supabase(PostgreSQL) `todo_tbl` 테이블에 사용자별로 저장되어 기기/브라우저와 관계없이 데이터 유지

## 실행 방법

별도의 설치 없이 `index.html` 파일을 브라우저에서 열면 바로 사용할 수 있습니다. (인터넷 연결이 필요합니다)

```bash
# 예시 (원하는 방식으로 열면 됩니다)
start index.html      # Windows
open index.html       # macOS
```

## 기술 스택

- HTML5
- CSS3 (Flexbox, Grid, CSS Variables)
- Vanilla JavaScript (ES6+)
- [Supabase](https://supabase.com) (PostgreSQL + Auth + supabase-js) — 인증 및 데이터 저장/조회

## 데이터베이스 구조

**`user_tbl`** — `auth.users`와 1:1로 연결되는 사용자 프로필 테이블

| 컬럼         | 타입        | 설명                                   |
| ------------ | ----------- | -------------------------------------- |
| `id`         | uuid (PK)   | `auth.users.id`를 참조 (FK, on delete cascade) |
| `email`      | text        | 가입 이메일                            |
| `created_at` | timestamptz | 생성 시각 (기본값 now())               |

회원가입(`auth.signUp`) 시 `auth.users`에 트리거(`on_auth_user_created`)가 걸려 있어 `user_tbl`에 자동으로 프로필 행이 생성됩니다.

**`todo_tbl`** — `user_tbl`과 1:N 관계

| 컬럼         | 타입        | 설명                                     |
| ------------ | ----------- | ---------------------------------------- |
| `id`         | uuid (PK)   | 기본 키, 자동 생성                       |
| `user_id`    | uuid (FK)   | `user_tbl.id` 참조, 기본값 `auth.uid()`  |
| `text`       | text        | 할 일 내용                               |
| `category`   | text        | 개인 / 공부 / 업무 / 취미 중 하나        |
| `completed`  | boolean     | 완료 여부 (기본값 false)                 |
| `created_at` | timestamptz | 생성 시각 (기본값 now())                 |

두 테이블 모두 Row Level Security가 활성화되어 있고, `auth.uid() = user_id` (또는 `= id`) 조건으로 로그인한 본인의 데이터만 조회/수정/삭제할 수 있습니다.

> **참고**: Supabase 프로젝트의 이메일 확인(Confirm email) 옵션이 켜져 있는 경우, 회원가입 후 이메일 인증을 완료해야 로그인할 수 있습니다. 또한 무료 플랜의 기본 이메일 발송에는 시간당 발송 제한이 있어 짧은 시간에 여러 계정을 연속 가입하면 "email rate limit exceeded" 오류가 날 수 있습니다.
