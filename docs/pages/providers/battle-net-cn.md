---
title: "Battle.net"
---

# Battle.net CN

OAuth 2.0 provider for Battle.net CN.

Also see the [OAuth 2.0](/guides/oauth2) guide.

## Initialization

```ts
import { BattleNet } from "arctic";

const battleNet = new BattleNet(clientId, clientSecret, redirectURI);
```

## Create authorization URL

```ts
import { generateState } from "arctic";

const state = generateState();
const url = bitBucket.createAuthorizationURL(state);
```

## Validate authorization code

`validateAuthorizationCode()` will either return an [`OAuth2Tokens`](/reference/main/OAuth2Tokens), or throw one of [`OAuth2RequestError`](/reference/main/OAuth2RequestError), [`ArcticFetchError`](/reference/main/ArcticFetchError), or a standard `Error` (parse errors). Battle.net returns an access token and a refresh token.

```ts
import { OAuth2RequestError, ArcticFetchError } from "arctic";

try {
	const tokens = await battleNet.validateAuthorizationCode(code);
	// Accessing other fields will throw an error
	const accessToken = tokens.accessToken();
	const refreshToken = tokens.refreshToken();
} catch (e) {
	if (e instanceof OAuth2RequestError) {
		// Invalid authorization code, credentials, or redirect URI
		const code = e.code;
		// ...
	}
	if (e instanceof ArcticFetchError) {
		// Failed to call `fetch()`
		const cause = e.cause;
		// ...
	}
	// Parse error
}
```

## Refresh access tokens

Use `refreshAccessToken()` to get a new access token using a refresh token. This method's behavior is identical to `validateAuthorizationCode()`.

```ts
import { OAuth2RequestError, ArcticFetchError } from "arctic";

try {
	const tokens = await battleNet.refreshAccessToken(refreshToken);
	// Accessing other fields will throw an error
	const accessToken = tokens.accessToken();
	const refreshToken = tokens.refreshToken();
} catch (e) {
	if (e instanceof OAuth2RequestError) {
		// Invalid authorization code, credentials, or redirect URI
	}
	if (e instanceof ArcticFetchError) {
		// Failed to call `fetch()`
	}
	// Parse error
}
```

## Get user profile

Enable the `account` scope on your account page and use the [`/userinfo` endpoint](https://develop.battle.net/documentation/battle-net/oauth-apis-cn).

```ts
const response = await fetch("https://oauth.battle.com.cn/oauth/userinfo", {
	headers: {
		Authorization: `Bearer ${accessToken}`
	}
});
const user = await response.json();
```
