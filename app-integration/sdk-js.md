# SDK Integration Guide

The Stackure JavaScript/TypeScript SDK provides the simplest way to integrate passwordless authentication, session validation, and role-based access directly into your app.

---

## Installation

```bash
npm install stackure
```

---

## Step 1. Trigger the sign-in flow

You can either redirect to the built-in Stackure sign-in UI or collect the user’s email yourself.

### Option A. Redirect to Stackure's UI

```js
import { signIn } from 'stackure';

await signIn('YOUR_APP_ID');   // Redirects the browser to Stackure's sign-in page
```

### Option B. Use your own email input

```js
import { signIn } from 'stackure';

await signIn('YOUR_APP_ID', 'user@example.com');
```

Stackure will email a magic link to the user.

---

## Step 2. Handle the redirect back to your app

After the user clicks the magic link, Stackure redirects to your app's registered URL with a `session_token` query parameter:

```
https://myapp.com?session_token=abc123...
```

**Your backend must:**

1. Extract the `session_token` from the URL query parameter
2. Set it as an HTTP-only cookie on your domain

**Example cookie settings:**
- Name: `session` (or your choice)
- Value: The `session_token` from URL
- HttpOnly: `true`
- Secure: `true`
- SameSite: `lax`
- Max-Age: `604800` (7 days)

This ensures session validation happens server-side and prevents token leakage to frontend code.

The SDK will automatically include this cookie when validating sessions.

---

## Step 3. Protect your routes

The SDK validates sessions using the cookie you set in Step 2.

### Automatic validation with middleware

```js
import { auth } from 'stackure';

// Basic authentication
const protectedHandler = auth({ appId: 'YOUR_APP_ID' });

// With role requirements
const adminHandler = auth({ appId: 'YOUR_APP_ID', roles: ['admin'] });
```

The middleware validates the session and injects the user object into the request.

---

### Manual validation

```js
import { verify } from 'stackure';

const result = await verify({ appId: 'YOUR_APP_ID' });

if (!result.authenticated) {
  // Handle unauthenticated: result.error.code, result.error.message
  return;
}

// Access user data: result.user
```

---

### Direct session check

```js
import { validateSession } from 'stackure';

const session = await validateSession('YOUR_APP_ID');

if (session.authenticated) {
  console.log('User:', session.user);
} else {
  console.log('Sign in at:', session.sign_in_url);
}
```

---

## Sign out

```js
import { logout } from 'stackure';

await logout();
```

---

**Prefer full control?** See the custom [API Integration Guide](api-integration.md).
