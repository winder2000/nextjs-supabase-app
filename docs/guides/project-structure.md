# 프로젝트 구조 가이드

이 문서는 Next.js + Supabase 스타터킷 기반 프로젝트의 폴더 구조, 파일 조직 및 네이밍 컨벤션을 정의합니다.

> **참고**: 이 프로젝트는 `src/` 디렉토리를 사용하지 않는다. `app/`, `components/`, `lib/`가 프로젝트 루트에 바로 위치한다.

## 🏗️ 전체 프로젝트 구조

```
nextjs-supabase-app/
├── docs/                   # 📚 프로젝트 문서
│   └── guides/            # 개발 가이드 모음
├── public/                # 🌍 정적 파일 (이미지, 아이콘)
├── app/                   # 🚀 Next.js App Router
├── components/            # 🧩 React 컴포넌트
├── lib/                   # 🛠️ 유틸리티 및 Supabase 클라이언트
├── components.json        # shadcn/ui 설정
├── next.config.ts         # Next.js 설정
├── package.json           # 의존성 및 스크립트
├── tsconfig.json          # TypeScript 설정
├── proxy.ts               # 미들웨어 (Next.js 16부터 middleware.ts → proxy.ts)
└── CLAUDE.md              # 개발 지침 메인 문서
```

## 📁 세부 폴더 구조

### app/ - App Router 페이지

```
app/
├── layout.tsx              # 🎨 루트 레이아웃 (전역 설정)
├── page.tsx                # 🏠 홈페이지 (/)
├── globals.css             # 🎨 전역 CSS 스타일
├── favicon.ico              # 🔖 파비콘
├── opengraph-image.png      # OG 이미지
├── twitter-image.png        # 트위터 카드 이미지
├── auth/                    # 🔐 인증 관련 페이지
│   ├── login/page.tsx
│   ├── sign-up/page.tsx
│   ├── sign-up-success/page.tsx
│   ├── forgot-password/page.tsx
│   ├── update-password/page.tsx
│   ├── error/page.tsx
│   └── confirm/route.ts     # 이메일 확인 Route Handler
├── protected/                # 🔒 인증 필요 라우트
│   ├── layout.tsx
│   └── page.tsx
└── instruments/page.tsx      # 예제/튜토리얼 페이지
```

**🚀 App Router 규칙:**

- `page.tsx`: 해당 경로의 메인 페이지
- `layout.tsx`: 레이아웃 컴포넌트 (자식 페이지 감쌈)
- `route.ts`: API Route Handler
- `loading.tsx`: 로딩 UI (필요시)
- `error.tsx`: 에러 UI (필요시)
- `not-found.tsx`: 404 페이지 (필요시)

### components/ - 컴포넌트 조직

```
components/
├── ui/                       # 🎛️ 기본 UI 컴포넌트 (shadcn/ui)
│   ├── button.tsx
│   ├── card.tsx
│   ├── badge.tsx
│   ├── checkbox.tsx
│   ├── dropdown-menu.tsx
│   ├── input.tsx
│   └── label.tsx
├── tutorial/                 # 📖 스타터킷 튜토리얼 컴포넌트
│   ├── code-block.tsx
│   ├── connect-supabase-steps.tsx
│   ├── fetch-data-steps.tsx
│   ├── sign-up-user-steps.tsx
│   └── tutorial-step.tsx
├── auth-button.tsx           # 🔐 로그인/로그아웃 상태 버튼
├── login-form.tsx            # 로그인 폼
├── sign-up-form.tsx          # 회원가입 폼
├── forgot-password-form.tsx  # 비밀번호 찾기 폼
├── update-password-form.tsx  # 비밀번호 변경 폼
├── logout-button.tsx         # 로그아웃 버튼
├── theme-switcher.tsx        # 🌓 다크모드 토글 (next-themes)
├── deploy-button.tsx         # 배포 버튼 (스타터킷 기본 제공)
├── env-var-warning.tsx       # 환경변수 미설정 경고
├── hero.tsx                  # 홈페이지 히어로 섹션
└── next-logo.tsx / supabase-logo.tsx  # 로고 컴포넌트
```

이 프로젝트에는 `layout/`, `navigation/`, `sections/`, `providers/` 같은 하위 카테고리 폴더가 없다. 컴포넌트 대부분이 `components/` 바로 아래에 평탄하게 위치하며, `ui/`와 `tutorial/`만 별도 폴더로 분리되어 있다.

### lib/ - 유틸리티 및 Supabase 클라이언트

```
lib/
├── utils.ts                    # 🛠️ 공통 유틸리티 함수 (cn 등)
└── supabase/
    ├── client.ts                # 브라우저용 클라이언트 (createBrowserClient)
    ├── server.ts                 # 서버용 클라이언트 (Server Components/Route Handlers)
    ├── proxy.ts                  # 미들웨어 세션 갱신 로직 (updateSession)
    └── database.types.ts         # Supabase CLI로 생성된 타입 (수동 편집 금지)
```

`env.ts`, `constants.ts`, `hooks/`, `schemas/`, `api/` 등은 이 프로젝트에 존재하지 않는다. 필요해지면 그때 추가한다.

## 🏷️ 파일 네이밍 컨벤션

### 파일명 규칙

```bash
# ✅ 올바른 파일명
user-profile.tsx        # kebab-case (권장, 이 프로젝트의 실제 컨벤션)

# ❌ 잘못된 파일명
user_profile.tsx        # snake_case (금지)
userprofile.tsx         # 소문자만 (금지)
```

### 컴포넌트 네이밍

```typescript
// ✅ 올바른 컴포넌트 네이밍
export function LoginForm() {} // PascalCase

// ❌ 잘못된 컴포넌트 네이밍
export function loginForm() {} // camelCase (금지)
```

### 폴더 네이밍

```bash
# ✅ 올바른 폴더명
components/             # 소문자
auth/                   # 소문자
```

## 🔗 경로 별칭 (Path Aliases)

`tsconfig.json`에서 `@/*`를 프로젝트 루트로 매핑한다 (`src/` 없음):

```json
"paths": { "@/*": ["./*"] }
```

```typescript
// ✅ 경로 별칭 사용 (권장)
import { Button } from '@/components/ui/button'
import { cn } from '@/lib/utils'
import { createClient } from '@/lib/supabase/server'

// ❌ 상대 경로 사용 (금지)
import { Button } from '../../../components/ui/button'
```

**📍 `components.json`에 정의된 별칭:**

- `@/components` → `components`
- `@/lib` → `lib`
- `@/components/ui` → `components/ui`
- `@/lib/utils` → `lib/utils`
- `@/hooks` → `hooks` *(별칭만 정의되어 있고 실제 `hooks/` 폴더는 아직 없음)*

## 📝 새 파일/폴더 추가 규칙

### 1. 새 UI 컴포넌트 추가

```bash
# shadcn/ui 컴포넌트 추가
npx shadcn@latest add [component-name]

# 커스텀 UI 컴포넌트 추가
components/ui/custom-component.tsx
```

### 2. 새 페이지 추가

```bash
# 정적 페이지
app/about/page.tsx

# 동적 페이지
app/users/[id]/page.tsx
```

### 3. 새 비즈니스 컴포넌트 추가

```bash
# 위치 결정 기준:
1. 특정 페이지에서만 사용 → 해당 페이지 폴더 내
2. 여러 페이지에서 사용 → components/ 바로 아래 또는 성격이 비슷한 컴포넌트 묶어 하위 폴더 신설
```

### 4. 새 유틸리티 추가

```bash
# 공통 유틸리티
lib/utils.ts            # 기존 파일에 추가

# Supabase 관련
lib/supabase/            # 클라이언트/서버 클라이언트 외 로직 추가 시 이 폴더 아래
```

## 🎯 코드 조직 베스트 프랙티스

### 1. 단일 책임 원칙

- 하나의 파일은 하나의 주요 기능만 담당
- 관련된 타입과 유틸리티는 같은 파일에 포함 가능

### 2. 의존성 순서

```typescript
// 1. 외부 라이브러리
import { NextPage } from 'next'

// 2. 내부 라이브러리 (@/ 경로)
import { Button } from '@/components/ui/button'
import { cn } from '@/lib/utils'

// 3. 상대 경로
import './component.css'
```

### 3. Export 규칙

```typescript
// ✅ Named export 사용 (권장)
export function LoginForm() {}

// ✅ Default export (페이지 컴포넌트)
export default function LoginPage() {}
```

### 4. 파일 크기 관리

- 단일 파일: 300줄 이하 권장
- 300줄 초과 시 분할 고려

## 🚫 금지사항

### ❌ 피해야 할 구조

```bash
# 깊은 중첩 구조 (4단계 이상)
components/pages/auth/forms/login/LoginForm.tsx

# 의미 없는 폴더명
components/misc/
components/common/

# 혼재된 케이스
components/Auth/userProfile.tsx
```

### ❌ 피해야 할 패턴

```typescript
// 혼재된 import
import Button from '@/components/ui/button' // default
import { Card } from '@/components/ui/card' // named

// 깊은 상대 경로
import { utils } from '../../../../lib/utils'
```

## ✅ 체크리스트

새 파일/폴더 추가 시 확인사항:

- [ ] `src/` 없이 루트 기준 경로로 배치했는가
- [ ] kebab-case 파일명 사용
- [ ] PascalCase 컴포넌트명 사용
- [ ] 경로 별칭(`@/`) 사용
- [ ] 단일 책임 원칙 준수
- [ ] 파일 크기 300줄 이하 유지
