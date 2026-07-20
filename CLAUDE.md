# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 프로젝트 개요

Next.js 15 (App Router) + Supabase 스타터킷 기반 프로젝트. `src/` 디렉토리 없이 루트에 `app/`, `components/`, `lib/`가 바로 위치한 구조다.

## 커맨드

```bash
npm run dev      # 개발 서버
npm run build    # 프로덕션 빌드
npm run start    # 프로덕션 서버 실행
npm run lint     # eslint .
```

테스트 스크립트는 정의되어 있지 않다. 변경 검증은 `npm run lint`와 `npm run build`로 한다.

## 구조상 특이사항

- **`proxy.ts`가 미들웨어 역할을 한다.** Next.js 16부터 `middleware.ts`가 `proxy.ts`로 개명되었고, 이 프로젝트는 그 규칙을 따른다. 세션 갱신 로직은 `lib/supabase/proxy.ts`의 `updateSession`에 있다.
- `next.config.ts`에 `cacheComponents: true`(실험적 캐시 기능)와 `allowedDevOrigins`(특정 LAN IP 허용)가 설정되어 있다.
- Supabase 클라이언트는 `lib/supabase/client.ts`(브라우저), `lib/supabase/server.ts`(서버)로 분리되어 있다. `lib/supabase/database.types.ts`는 Supabase에서 생성한 타입이므로 수동으로 편집하지 않는다.
- `supabase/` 디렉토리(migrations, config.toml 등)는 없다 — 로컬 CLI 스택 없이 원격 Supabase 프로젝트만 사용한다.
- `docs/guides/`의 문서는 이 프로젝트의 실제 구조(`src/` 없음)에 맞게 수정되어 있다.
- Git Hooks(Husky pre-commit 등)는 설정되어 있지 않다 — 커밋 전 자동 lint/format이 없으므로 커밋 전 `npm run lint`를 직접 실행해야 한다.

### Supabase 클라이언트 3종 사용 패턴

1. **Server Components / Route Handlers**: `lib/supabase/server.ts`의 `createClient()`
   - **중요**: Fluid compute 환경 대응을 위해 함수 내부에서 매 요청마다 새로 생성해야 한다 (전역 변수로 캐싱 금지)
   ```typescript
   import { createClient } from "@/lib/supabase/server";

   export default async function ServerComponent() {
     const supabase = await createClient();
     const { data } = await supabase.from("table").select();
   }
   ```

2. **Client Components**: `lib/supabase/client.ts`의 `createClient()` (`createBrowserClient` 기반)
   ```typescript
   "use client";
   import { createClient } from "@/lib/supabase/client";

   export default function ClientComponent() {
     const supabase = createClient();
     // ...
   }
   ```

3. **`proxy.ts` (미들웨어)**: `lib/supabase/proxy.ts`의 `updateSession()`
   - `createServerClient`와 `supabase.auth.getClaims()` 사이에 코드를 추가하지 말 것 — 세션이 랜덤하게 끊기는 디버깅 어려운 버그의 원인이 된다.
   - 새 `NextResponse` 객체를 만들 경우 반드시 요청 쿠키와 응답 쿠키를 모두 복사할 것.

### 인증 흐름

- `proxy.ts`(matcher: 정적 파일 제외 전 경로)가 모든 요청을 가로채 인증 상태를 확인한다.
- `/protected` 경로는 인증된 사용자만 접근 가능하다.
- `/`, `/login`, `/auth/*` 경로는 미인증 상태에서도 접근 가능하다(리다이렉트 예외).
- `app/auth/confirm/route.ts`가 이메일 확인(OTP) 처리를 담당한다.
- 환경 변수(`NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY`)가 없으면 `proxy.ts`는 세션 체크를 자동으로 건너뛴다(`hasEnvVars` 체크).

## 커밋 컨벤션

`.claude/commands/git/commit.md`는 이모지+컨벤셔널 커밋 형식(`✨ feat: ...`)을 규정하지만, 실제 커밋 로그는 **이모지 없이 한글 서술형 제목 한 줄 + 상세 본문**을 사용한다. 커밋 메시지를 작성할 때는 실제 관행을 따른다.

## 환경 변수

`.env.local`에 `NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY` 두 개만 필요하다.

## MCP 서버 설정 (`.mcp.json`)

- **supabase**: 이 프로젝트의 Supabase 원격 프로젝트(project_ref: `vkvqyxrziwitexvhkevr`)에 연동
- **playwright**: 브라우저 자동화
- **context7**: 라이브러리 문서 검색
- **sequential-thinking**: 단계적 사고 보조
- **shadcn**: shadcn/ui 컴포넌트 관리
- **shrimp-task-manager**: 작업 관리 (`shrimp_data/`에 데이터 저장)
