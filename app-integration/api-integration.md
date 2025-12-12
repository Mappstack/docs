# API Integration Guide

This guide shows how to integrate Stackure authentication and session validation directly using the REST API.   
Use this path if you need full control over your authentication pipeline instead of using the SDK.

---

## Step 1. Send a magic sign-in link

Trigger a magic-link email for your user.

```bash
curl -X POST https://stackure.com/api/public/auth/magic-link/send \
  -H "Content-Type: application/json" \
  -d '{
    "user_email": "user@example.com",
    "app_id": "YOUR_APP_ID"
  }'
```

**Response**
```json
{
  "message": "if the email exists, a sign in link will be sent."
}
```

---

## Step 2. Handle the magic-link redirect

After the user clicks the sign-in link, Stackure redirects them to your app's registered URL with a `session_token` query parameter.

```
https://myapp.com?session_token=abc123...
```

Extract the `session_token` and store it. The examples below use Bearer token authentication, but HTTP-only cookies are also supported.

---

## Step 3. Validate the session on each protected request

Your backend must validate the `session_token` before serving protected data.

**Request**
```bash
curl "https://stackure.com/api/public/auth/session/validate?app_id=YOUR_APP_ID" \
  -H "Authorization: Bearer SESSION_TOKEN"
```

**Response if the session is valid**
```json
{
  "authenticated": true,
  "user": {
    "user_id": "uuid",
    "user_email": "user@example.com",
    "user_first_name": "John",
    "user_last_name": "Doe",
    "user_roles": ["admin", "developer"]
  }
}
```

**Response if the session is invalid or expired**
```json
{
  "authenticated": false,
  "sign_in_url": "https://stackure.com/sign-in/magic-link?app_id=YOUR_APP_ID"
}
```

---

**Prefer a simpler setup?** See the [SDK Guide](sdk-js.md).