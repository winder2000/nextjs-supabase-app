---
name: verify-app
description: 코드 변경 후 lint와 build로 검증한다. 이 프로젝트에는 테스트 스크립트가 없으므로 코드 수정을 마쳤을 때, 커밋 전, 또는 사용자가 "검증해줘" / "확인해줘" 라고 요청할 때 사용한다.
---

이 프로젝트(Next.js 15 App Router + Supabase)에는 `npm test` 같은 테스트 스크립트가 없다. 대신 다음 순서로 검증한다.

1. `npm run lint` 실행 — eslint 오류/경고 확인
2. `npm run build` 실행 — 타입 오류, 빌드 실패 여부 확인
3. 둘 다 통과하면 변경사항이 구조적으로 안전하다고 판단한다
4. UI/프론트엔드 변경이라면 `npm run dev`로 개발 서버를 띄우고 실제 브라우저에서 동작을 확인한다 (lint/build 통과만으로는 기능 정확성을 보장하지 않는다)

실패 시 원인을 파악해 수정하고, 다시 lint → build 순서로 재검증한다.
