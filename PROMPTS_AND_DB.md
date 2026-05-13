# 📝 프롬프트 및 데이터베이스 명세서 (Prompts & DB Specs)

이 문서는 프로젝트에서 사용되는 **AI 프롬프트(Prompts)**와 **데이터베이스(DB) 스키마/설정**을 정리하고 관리하기 위한 공간입니다. 아래의 템플릿을 필요에 맞게 수정하여 사용해 주세요.

---

## 1. 🤖 AI 프롬프트 (Prompts)

이곳에 AI 모델(예: ChatGPT, Gemini 등)에게 전달할 시스템 프롬프트(System Prompt), 템플릿, 그리고 활용 목적을 기록하세요.

### 1.1. [기능명] 프롬프트 템플릿
- **목적**: (예: 멘토와 멘티의 성향을 분석하여 매칭 추천)
- **사용 모델**: (예: GPT-4o, Gemini 1.5 Pro)
- **System Prompt**: 
  ```text
  당신은 전문적인 멘토링 매칭 매니저입니다. 
  주어진 멘토의 정보와 멘티의 요구사항을 분석하여...
  ```
- **User Prompt 템플릿**:
  ```text
  [멘토 정보]: {{mentor_data}}
  [멘티 요구사항]: {{mentee_data}}
  위 정보를 바탕으로 두 사람의 매칭 적합도를 100점 만점으로 평가하고 이유를 설명해주세요.
  ```

---

## 2. 🗄️ 데이터베이스 스키마 (Database Schema)

이곳에 데이터베이스 모델(테이블), 필드 속성, 그리고 테이블 간의 관계(Relations)를 기록하세요. (예: PostgreSQL, MongoDB, MySQL 등)

### 2.1. Users (사용자) 테이블
사용자의 기본 정보와 멘토/멘티 역할을 저장합니다.

| 필드명 (Field) | 데이터 타입 (Type) | 제약조건 (Constraints) | 설명 (Description) |
| :--- | :--- | :--- | :--- |
| `id` | UUID | Primary Key | 사용자 고유 식별자 |
| `email` | String | Unique, Not Null | 사용자 이메일 (로그인) |
| `password` | String | Not Null | 해시된 비밀번호 |
| `name` | String | Not Null | 사용자 실명 또는 닉네임 |
| `role` | Enum | `MENTOR`, `MENTEE` | 사용자의 역할 |
| `created_at` | DateTime | Default: Now() | 계정 생성 일시 |

### 2.2. Activities (활동/과제) 테이블
멘토가 부여하거나 멘티가 수행하는 과제 목록을 저장합니다.

| 필드명 (Field) | 데이터 타입 (Type) | 제약조건 (Constraints) | 설명 (Description) |
| :--- | :--- | :--- | :--- |
| `id` | UUID | Primary Key | 활동 고유 식별자 |
| `user_id` | UUID | Foreign Key | 해당 활동을 수행하는 사용자 |
| `title` | String | Not Null | 활동 제목 |
| `status` | Enum | `TODO`, `IN_PROGRESS`, `DONE` | 활동 진행 상태 |
| `due_date` | DateTime | Nullable | 마감 기한 |

---

## 3. ⚙️ 데이터베이스 연결 및 설정 (DB Configuration)
데이터베이스 종류나 접속 환경 변수(Environment Variables) 규칙을 메모해 둘 수 있습니다.

- **DB 종류**: (예: Supabase PostgreSQL)
- **환경 변수**:
  - `DATABASE_URL`: `postgresql://user:password@localhost:5432/mentorbridge`
