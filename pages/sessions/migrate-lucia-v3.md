---
title: "Lucia v3에서 마이그레이션"
---

# Lucia v3에서 마이그레이션

Lucia v3는 가볍고 비교적 낮은 수준이기 때문에 프로젝트를 마이그레이션하는 데 오래 걸리지 않습니다. 게다가 대부분의 지식은 여전히 매우 유용합니다. 데이터베이스 마이그레이션은 필요하지 않습니다.

세션에 대한 API는 [기본 세션 API](/sessions/basic-api)페이지에서 다루고 있습니다.

- `Lucia.createSession()` => `generateSessionToken()` and `createSession()`
- `Lucia.validateSession()` => `validateSessionToken()`
- `Lucia.invalidateSession()` => `invalidateSession()`

쿠키에 대한 API는 [세션 쿠키](/sessions/cookies)페이지에서 다룹니다.

- `Lucia.createSessionCookie()` => `setSessionTokenCookie()`
- `Lucia.createBlankSessionCookie()` => `deleteSessionTokenCookie()`

세션 작동 방식의 한 가지 변경 사항은 이제 세션 토큰이 저장되기 전에 해싱된다는 것입니다. 사전 해싱 토큰은 클라이언트에서 할당된 세션 ID이고 해시는 내부 세션 ID입니다. 가장 쉬운 방법은 기존 세션을 모두 삭제하는 것이지만, 기존 세션을 유지하려면 데이터베이스에 저장된 세션 ID를 SHA-256 및 16진수 인코딩합니다. 또는 해싱을 완전히 건너뛸 수도 있습니다. 해싱은 데이터베이스 유출을 방지하는 좋은 방법이지만 반드시 필요한 것은 아닙니다.

```ts
export function createSession(userId: number): Session {
	const bytes = new Uint8Array(20);
	crypto.getRandomValues(bytes);
	const sessionId = encodeBase32LowerCaseNoPadding(bytes);
	// Insert session into database.
	return session;
}

export function validateSessionToken(sessionId: string): SessionValidationResult {
	// Get and validate session
	return { session, user };
}
```

도움이 필요하거나 궁금한 점이 있으면 [Discord](https://discord.com/invite/PwrK3kpVR3) 또는 [GitHub](https://github.com/lucia-auth/lucia/discussions)에서 문의해 주세요.
