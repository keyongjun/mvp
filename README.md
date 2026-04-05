# 개인 개발 블로그 (Notion CMS)

Notion을 CMS로 활용한 개인 기술 블로그입니다. Notion에서 글을 작성하면 자동으로 블로그에 반영됩니다.

## 기술 스택

- **Frontend**: Next.js 15, TypeScript
- **CMS**: Notion API (`@notionhq/client`)
- **Styling**: Tailwind CSS, shadcn/ui
- **Icons**: Lucide React
- **Deployment**: Vercel

## 시작하기

### 1. 패키지 설치

```bash
npm install
```

### 2. 환경 변수 설정

`.env.local` 파일을 생성하고 아래 값을 입력합니다.

```env
NOTION_API_KEY=secret_xxxxxxxxxxxx
NOTION_DATABASE_ID=xxxxxxxxxxxxxxxxxxxx
```

Notion Integration 생성: https://www.notion.so/my-integrations

### 3. 개발 서버 실행

```bash
npm run dev
```

## 주요 기능

- Notion 데이터베이스에서 블로그 글 목록 조회
- 개별 글 상세 페이지
- 카테고리별 필터링
- 제목/태그 기반 검색
- 반응형 디자인

## Notion 데이터베이스 구조

| 필드      | 타입         | 설명              |
| --------- | ------------ | ----------------- |
| Title     | title        | 글 제목           |
| Category  | select       | 카테고리          |
| Tags      | multi_select | 태그              |
| Published | date         | 발행일            |
| Status    | select       | `초안` / `발행됨` |

## 문서

- [PRD](./docs/PRD.md)
