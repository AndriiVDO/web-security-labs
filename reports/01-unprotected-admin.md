# Unprotected Admin Functionality

## Environment

PortSwigger Web Security Academy — intentionally vulnerable training lab.

Lab: https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality

## Summary

The application allowed access to an administrative panel without authentication.
The panel also allowed deletion of a user account.

## Steps to reproduce

1. Launch the lab without logging in.
2. Open `/robots.txt`.
3. Find `Disallow: /administrator-panel`.
4. Open `/administrator-panel`.
5. Delete the lab user `carlos`.

## Expected behavior

The server should require authentication and administrator permissions
before granting access to the panel or allowing user deletion.

## Observed behavior

The administrative panel and deletion action were accessible without login.

## Impact

An unauthenticated visitor could delete another user's account.

## Recommended fix

- Require authentication for administrative routes.
- Check administrator permissions on every administrative request.
- Apply the same checks to the user deletion endpoint.
- Deny access by default.
- Do not treat hidden links or robots.txt as access controls.

## What I learned

Authentication identifies the user.
Authorization determines which actions the user may perform.

Knowing an administrative URL must not grant administrative permissions.
