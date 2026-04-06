# ROADMAP: 개인 개발 블로그 (Notion CMS)

> PRD 기반 개발 로드맵. 각 Phase는 순서대로 진행하며, 이전 Phase 완료 기준을 충족한 후 다음 Phase로 넘어간다.

## 전체 일정 요약

| Phase   | 내용               | 예상 소요 | 누적   |
| ------- | ------------------ | --------- | ------ |
| Phase 1 | 프로젝트 초기 설정 | 1~2일     | 1~2일  |
| Phase 2 | 공통 모듈 개발     | 2~3일     | 3~5일  |
| Phase 3 | 핵심 기능 개발     | 3~4일     | 6~9일  |
| Phase 4 | 추가 기능 개발     | 2~3일     | 8~12일 |
| Phase 5 | 최적화 및 배포     | 1~2일     | 9~14일 |

---

## Phase 1: 프로젝트 초기 설정

**예상 소요:** 1~2일

**배경:** 견고한 기반 없이는 기능 개발이 어렵다. Next.js 앱 구조와 Notion API 연결을 먼저 확립한다.

### 작업 목록

- [ ] Next.js 15 프로젝트 생성 (`create-next-app`, TypeScript + Tailwind CSS)
- [ ] shadcn/ui 초기화 및 기본 컴포넌트 설치
- [ ] Notion Integration 생성 및 API 키 발급
- [ ] Notion 블로그 데이터베이스 생성 (Title, Category, Tags, Published, Status)
- [ ] `.env.local` 환경 변수 설정 (`NOTION_API_KEY`, `NOTION_DATABASE_ID`)
- [ ] `@notionhq/client` 패키지 설치
- [ ] 기본 레이아웃 구조 생성 (`app/layout.tsx`, Header, Footer 플레이스홀더)
- [ ] 테스트용 샘플 글 3건 이상 Notion DB에 작성 (Status: 발행됨)

### 완료 기준

- `npm run dev` 실행 시 오류 없이 로컬 서버가 구동된다.
- Notion API 호출 테스트 스크립트(`scripts/test-notion.ts`)가 DB 목록을 정상 반환한다.
- 샘플 데이터가 Notion DB에 존재한다.

---

## Phase 2: 공통 모듈 개발

**예상 소요:** 2~3일

**배경:** 모든 기능에서 재사용되는 코드를 먼저 만들어야 중복을 방지하고 이후 Phase의 개발 속도를 높일 수 있다.

### 작업 목록

**타입 정의** (`src/types/notion.ts`)

- [ ] `Post` 타입 (id, title, slug, category, tags, publishedAt, status)
- [ ] `Category` 타입
- [ ] Notion API 응답 타입 래퍼

**Notion API 유틸리티** (`src/lib/notion.ts`)

- [ ] `getPosts()` — 발행된 글 전체 목록 조회 (Status = 발행됨, 최신순)
- [ ] `getPostBySlug(slug)` — 슬러그로 특정 글 조회
- [ ] `getPostsByCategory(category)` — 카테고리별 글 목록 조회
- [ ] `getPageBlocks(pageId)` — 글 본문 블록 조회
- [ ] `getCategories()` — 전체 카테고리 목록 조회

**공통 컴포넌트** (`src/components/`)

- [ ] `Header` — 블로그명, 네비게이션
- [ ] `Footer` — 저작권 표시
- [ ] `PostCard` — 제목, 카테고리, 태그, 발행일 표시 카드

### 완료 기준

- `getPosts()` 호출 시 Notion DB의 발행된 글 목록이 `Post[]` 타입으로 반환된다.
- `getPageBlocks(pageId)` 호출 시 해당 페이지의 블록 배열이 반환된다.
- `PostCard`가 Storybook 또는 `/` 임시 페이지에서 샘플 데이터로 렌더링된다.
- TypeScript 컴파일 오류가 없다.

---

## Phase 3: 핵심 기능 개발

**예상 소요:** 3~4일

**배경:** 블로그의 가장 기본이 되는 기능이다. 글 목록과 상세 페이지 없이는 다른 기능이 의미 없다.

### 작업 목록

**글 목록 페이지** (`src/app/page.tsx`)

- [ ] `getPosts()`로 글 목록 서버 사이드 조회
- [ ] `PostCard` 그리드 레이아웃 렌더링
- [ ] 발행일 기준 최신순 정렬

**글 상세 페이지** (`src/app/posts/[slug]/page.tsx`)

- [ ] 동적 라우트 설정 및 `generateStaticParams` 구현
- [ ] 제목, 발행일, 카테고리, 태그 메타 정보 표시
- [ ] 목록으로 돌아가기 링크

**Notion 컨텐츠 렌더링** (`src/components/PostContent.tsx`)

- [ ] 단락(`paragraph`)
- [ ] 제목(`heading_1`, `heading_2`, `heading_3`)
- [ ] 글머리 기호(`bulleted_list_item`)
- [ ] 번호 목록(`numbered_list_item`)
- [ ] 코드 블록(`code`) — 언어 표시 및 구문 강조
- [ ] 인용(`quote`)
- [ ] 이미지(`image`)
- [ ] 구분선(`divider`)

### 완료 기준

- `/` 경로에서 발행된 글 목록이 카드 형태로 표시된다.
- `/posts/[slug]` 경로에서 해당 글의 제목, 메타 정보, 본문이 표시된다.
- 위 8가지 Notion 블록 타입이 오류 없이 렌더링된다.
- 존재하지 않는 slug 접근 시 404 페이지가 표시된다.

---

## Phase 4: 추가 기능 개발

**예상 소요:** 2~3일

**배경:** 핵심 기능이 완성된 후 UX를 개선하는 부가 기능을 추가한다. Phase 3 완료 없이는 시작하지 않는다.

### 작업 목록

**카테고리 필터링**

- [ ] `CategoryFilter` 컴포넌트 — 카테고리 탭 UI
- [ ] URL 파라미터 기반 상태 관리 (`?category=React`)
- [ ] `/category/[name]` 라우트 구현 (`src/app/category/[name]/page.tsx`)
- [ ] 필터 적용 시 해당 카테고리 글만 표시

**검색 기능**

- [ ] `SearchBar` 컴포넌트 — 검색어 입력창
- [ ] 제목 및 태그 기반 클라이언트 사이드 실시간 필터링
- [ ] 검색 결과 없을 시 빈 상태 UI 표시

**SEO 최적화**

- [ ] 각 페이지 `metadata` 설정 (title, description)
- [ ] 글 상세 페이지 OG 태그 (og:title, og:description, og:image)
- [ ] `sitemap.ts` 자동 생성
- [ ] `robots.txt` 설정

### 완료 기준

- 카테고리 탭 클릭 시 URL이 `?category=카테고리명`으로 변경되고 해당 글만 표시된다.
- 검색어 입력 시 제목 또는 태그가 일치하는 글만 실시간으로 필터링된다.
- `https://블로그URL/sitemap.xml` 접근 시 발행된 글 URL 목록이 반환된다.
- 글 상세 페이지의 OG 태그가 SNS 미리보기에서 정상 표시된다.

---

## Phase 5: 최적화 및 배포

**예상 소요:** 1~2일

**배경:** 기능이 완성된 후 성능과 품질을 높이고 실제 서비스를 배포한다.

### 작업 목록

**성능 최적화**

- [ ] Next.js ISR 설정 (`revalidate` 값 지정 — 권장: 60초)
- [ ] `next/image`로 이미지 최적화
- [ ] 불필요한 클라이언트 컴포넌트 서버 컴포넌트로 전환 검토

**반응형 디자인 개선**

- [ ] 모바일(320px~) 레이아웃 점검
- [ ] 태블릿(768px~) 레이아웃 점검
- [ ] 데스크톱(1024px~) 레이아웃 점검
- [ ] 코드 블록 모바일 가로 스크롤 처리

**Vercel 배포**

- [ ] Vercel 프로젝트 생성 및 GitHub 연동
- [ ] 환경 변수 (`NOTION_API_KEY`, `NOTION_DATABASE_ID`) 등록
- [ ] 프로덕션 배포 및 도메인 확인
- [ ] 배포 후 전체 기능 동작 확인

### 완료 기준

- Lighthouse 성능 점수 80점 이상 (모바일 기준)
- 모바일 / 태블릿 / 데스크톱 3개 뷰포트에서 레이아웃 깨짐 없음
- Vercel 프로덕션 URL에서 전체 기능이 정상 동작한다.
- Notion DB에 새 글 발행 후 최대 60초 내에 블로그에 반영된다.

---

## 이후 개선 사항 (Post-MVP)

MVP 완료 후 우선순위에 따라 순차 진행한다.

| 항목            | 설명                                   |
| --------------- | -------------------------------------- |
| 다크 모드       | `next-themes` 활용, 시스템 설정 연동   |
| RSS 피드        | `/feed.xml` 엔드포인트 생성            |
| 좋아요 / 북마크 | 로컬스토리지 기반 클라이언트 상태 관리 |
| 댓글 기능       | Giscus(GitHub Discussions) 연동        |
| 태그 페이지     | `/tag/[name]` 라우트 추가              |
| 글 목차(TOC)    | H2~H3 기반 자동 생성, 스크롤 연동      |
