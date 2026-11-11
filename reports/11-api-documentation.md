# Exploiting an API Endpoint Using Documentation

## Environment

- Platform: PortSwigger Web Security Academy
- Lab: Exploiting an API endpoint using documentation
- Difficulty: Apprentice
- Status: Solved
- Tools: Burp Suite Community Edition, Repeater, Chrome

## Summary

The application exposes REST API documentation that reveals available endpoints and HTTP methods.

During testing, an insecure DELETE endpoint was discovered that allowed an authenticated user to delete another user's account.

This demonstrates a broken object-level authorization vulnerability.

## Steps Performed

1. Opened the PortSwigger laboratory.
2. Logged in using the provided `wiener` test account.
3. Updated the account email to `test@example.com`.
4. Captured the `PATCH /api/user/wiener` request using Burp Suite.
5. Inspected the JSON request and response.
6. Navigated to `/api/` and discovered the REST API documentation.
7. Identified GET, PATCH, and DELETE operations.
8. Sent the PATCH request to Burp Repeater.
9. Modified the request to `DELETE /api/user/carlos`.
10. Removed the unnecessary request body and Content-Type header.
11. Sent the modified request.
12. Received `200 OK` with the message `User deleted`.
13. Confirmed that the laboratory status changed to **Solved**.

## API Endpoints Discovered

| Method | Endpoint               | Purpose                   |
| ------ | ---------------------- | ------------------------- |
| GET    | `/api/user/{username}` | Retrieve user information |
| PATCH  | `/api/user/{username}` | Update user email         |
| DELETE | `/api/user/{username}` | Delete user account       |

## Vulnerable Behavior

The API accepted a DELETE request targeting another user's account.

Example request:

```http
DELETE /api/user/carlos HTTP/2
```

The server returned:

```json
{
  "status": "User deleted"
}
```

**HTTP Status:** 200 OK

The server did not enforce sufficient object-level authorization for this operation.

## Expected Behavior

The application should verify that the authenticated user is authorized to perform the requested operation on the target account.

Unauthorized deletion attempts should be rejected with an appropriate response, such as `403 Forbidden`.

## Observed Behavior

An authenticated user was able to delete another user's account through an API endpoint discovered in the documentation.

**Final result: Lab Solved.**

## Impact

In a real-world application, this vulnerability could allow unauthorized users to perform destructive operations.

Potential consequences include:

- Unauthorized account deletion
- Loss of user data
- Business disruption
- Abuse of administrative functionality
- Broken object-level authorization (BOLA)

## Recommended Fix

- Enforce server-side authorization for every API endpoint.
- Verify user ownership or administrative permissions.
- Apply least-privilege access control.
- Protect destructive HTTP operations.
- Use consistent authorization middleware.
- Test endpoints against unauthorized access.
- Monitor suspicious API activity.

API documentation should also be managed appropriately, but hiding documentation alone is not a substitute for authorization.

## What I Learned

During this laboratory, I learned:

- How to discover REST API documentation.
- How to inspect API requests using Burp Suite.
- How HTTP methods such as GET, PATCH, and DELETE work.
- How to modify API requests using Repeater.
- How missing authorization checks can expose destructive operations.
- Why server-side access control is essential for API security.

## Evidence

Screenshots are stored in the `screenshots/` directory.

- `11-api-update-email.png` — Email updated successfully.
- `11-api-email-request.png` — PATCH request in Burp Suite.
- `11-api-documentation.png` — REST API documentation.
- `11-api-delete-user.png` — Successful DELETE request.
- `11-api-solved.png` — Successfully completed laboratory.
