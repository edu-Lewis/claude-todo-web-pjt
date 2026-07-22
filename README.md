# 📝 Todo List

카테고리별로 할 일을 관리할 수 있는 간단한 웹 기반 Todo List 앱입니다. 별도의 빌드 과정이나 프레임워크 없이 순수 HTML/CSS/JavaScript로 작성되었습니다.

## 주요 기능

- **할 일 추가**: 텍스트와 카테고리(개인/공부/업무/취미)를 지정해 할 일을 추가
- **할 일 수정**: 인라인 편집으로 텍스트와 카테고리 변경
- **완료 처리**: 체크 버튼으로 완료 상태 토글
- **삭제**: 확인 후 할 일 삭제
- **카테고리 필터**: 탭을 눌러 전체/개인/공부/업무/취미별로 목록 확인
- **진행률 표시**: 전체 진행률과 카테고리별 진행률을 프로그레스 바로 표시
- **데이터 저장**: Supabase(PostgreSQL) `todo_tbl` 테이블에 저장되어 기기/브라우저와 관계없이 데이터 유지

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
- [Supabase](https://supabase.com) (PostgreSQL + supabase-js) — 데이터 저장/조회

## 데이터베이스 구조

`todo_tbl` 테이블 (Supabase Postgres):

| 컬럼         | 타입          | 설명                              |
| ------------ | ------------- | --------------------------------- |
| `id`         | uuid (PK)     | 기본 키, 자동 생성                |
| `text`       | text          | 할 일 내용                        |
| `category`   | text          | 개인 / 공부 / 업무 / 취미 중 하나 |
| `completed`  | boolean       | 완료 여부 (기본값 false)          |
| `created_at` | timestamptz   | 생성 시각 (기본값 now())          |

Row Level Security가 활성화되어 있으며, 앱은 공개(anon/publishable) 키로 CRUD를 수행합니다.
