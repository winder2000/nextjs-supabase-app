# Next.js + Supabase App

Next.js 15(App Router) + Supabase 인증 스타터킷 기반 프로젝트입니다. `src/` 디렉토리 없이 루트에 `app/`, `components/`, `lib/`가 바로 위치한 구조입니다.

## 기술 스택

- [Next.js](https://nextjs.org) 15 (App Router, Turbopack)
- [Supabase](https://supabase.com) (`@supabase/ssr`, `@supabase/supabase-js`) — 인증 및 데이터베이스
- [Tailwind CSS](https://tailwindcss.com)
- [shadcn/ui](https://ui.shadcn.com/) (Radix UI 기반)

## 시작하기

1. 의존성 설치

   ```bash
   npm install
   ```

2. `.env.local` 파일을 생성하고 Supabase 프로젝트 정보를 입력합니다.

   ```env
   NEXT_PUBLIC_SUPABASE_URL=[Supabase 프로젝트 URL]
   NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY=[Supabase publishable/anon key]
   ```

   두 값 모두 [Supabase 대시보드의 API 설정](https://supabase.com/dashboard/project/_?showConnect=true)에서 확인할 수 있습니다.

3. 개발 서버 실행

   ```bash
   npm run dev
   ```

   [localhost:3000](http://localhost:3000)에서 확인할 수 있습니다.

## 커맨드

```bash
npm run dev      # 개발 서버
npm run build    # 프로덕션 빌드
npm run start    # 프로덕션 서버 실행
npm run lint     # eslint .
```

별도의 테스트 스크립트는 없습니다. 변경 사항은 `npm run lint`와 `npm run build`로 검증합니다.

## 프로젝트 구조

```
app/
├── page.tsx                    # 홈
├── protected/page.tsx          # 인증된 사용자만 접근 가능한 페이지
├── instruments/page.tsx        # Supabase 테이블 조회 예시
└── auth/
    ├── login/page.tsx          # 로그인
    ├── sign-up/page.tsx        # 회원가입
    ├── sign-up-success/page.tsx
    ├── forgot-password/page.tsx
    ├── update-password/page.tsx
    ├── error/page.tsx
    └── confirm/route.ts        # 이메일 확인(OTP) 처리

components/                     # UI 컴포넌트 (components/ui는 shadcn/ui)
lib/supabase/
├── client.ts                   # 브라우저용 Supabase 클라이언트
├── server.ts                   # 서버용 Supabase 클라이언트
├── proxy.ts                    # 미들웨어 세션 갱신 로직
└── database.types.ts           # Supabase에서 생성한 DB 타입 (수동 편집 금지)

proxy.ts                        # Next.js 16 미들웨어 (구 middleware.ts)
```

## 인증 흐름

- 루트의 `proxy.ts`(Next.js 16부터 `middleware.ts`를 대체)가 정적 파일을 제외한 모든 요청을 가로채 인증 상태를 확인합니다.
- `/protected` 경로는 인증된 사용자만 접근할 수 있으며, 미인증 시 `/auth/login`으로 리다이렉트됩니다.
- `app/auth/confirm/route.ts`가 이메일 확인(OTP) 처리를 담당합니다.
- 환경 변수가 설정되지 않으면 세션 체크를 자동으로 건너뜁니다.

## 데이터베이스

원격 Supabase 프로젝트만 사용하며, 로컬 Supabase CLI 스택(`supabase/` 디렉토리)은 사용하지 않습니다. 현재 사용 중인 테이블은 다음과 같습니다.

- `instruments` — 조회 예시용 테이블
- `profiles` — 회원가입한 사용자의 프로필 정보
