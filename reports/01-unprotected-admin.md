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

## Verification with Burp Suite Repeater

I repeated the access control test using Burp Suite Community Edition.

- Sent the administrative panel request without a Cookie header.
- Confirmed that the response contained the user list and deletion links.
- Submitted the lab's user deletion request.
- Requested the panel again and confirmed that carlos was absent.
- Observed the message "User deleted successfully!".

This confirmed access to administrative functionality without authentication.
The verification checked the response content and resulting state,
rather than relying only on an HTTP status code.

## Additional observation

The application used GET for user deletion.

State-changing operations should use an appropriate HTTP method,
enforce authorization, and include CSRF protection when using cookie-based
authentication. Changing the HTTP method alone does not fix access control.
