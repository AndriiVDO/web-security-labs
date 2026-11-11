# Two-Factor Authentication (2FA) Simple Bypass

## Environment

- Platform: PortSwigger Web Security Academy
- Lab: 2FA simple bypass
- Difficulty: Apprentice
- Status: Solved
- Tools: Google Chrome, Chrome DevTools

## Summary

The application contains an authentication vulnerability that allows users to bypass two-factor authentication (2FA).

After successful password verification, the application redirects users to a second authentication step.

However, the server does not properly enforce completion of the second factor before granting access to protected account pages.

As a result, an attacker who knows valid first-factor credentials can directly access protected resources without providing the required security code.

## Steps Performed

1. Opened the PortSwigger Web Security Academy laboratory.
2. Logged in using the provided test account `wiener`.
3. Reached the `/login2` page requesting a 4-digit security code.
4. Did not enter the security code.
5. Manually changed the URL from `/login2` to `/my-account`.
6. Successfully accessed the account page without completing 2FA.
7. Logged out and repeated the test using the provided `carlos` account.
8. Bypassed the second authentication step using the same direct navigation technique.
9. Accessed the protected account page.
10. Confirmed that the laboratory status changed to **Solved**.

## Vulnerable Behavior

The application allows access to:

`/my-account`

without verifying that the user has successfully completed two-factor authentication.

The server treats successful password authentication as sufficient for access to protected resources.

## Expected Behavior

The application should require both authentication factors before granting access to protected pages.

Expected authentication flow:

1. Username and password verification.
2. Two-factor authentication verification.
3. Creation of a fully authenticated session.
4. Access to protected resources.

## Observed Behavior

The application granted access to the account page immediately after password authentication, even though the 2FA verification step had not been completed.

The vulnerability was confirmed using two laboratory accounts.

**Final result: Lab Solved.**

## Impact

In a real-world application, this vulnerability could allow attackers who have obtained valid passwords to bypass the additional security protection provided by 2FA.

Potential consequences include:

- Unauthorized account access.
- Exposure of personal information.
- Account takeover.
- Unauthorized modification of account settings.
- Reduced effectiveness of multi-factor authentication.

## Recommended Fix

- Enforce 2FA verification on the server.
- Track authentication state securely.
- Distinguish partially authenticated sessions from fully authenticated sessions.
- Restrict access to protected endpoints until all required authentication factors are verified.
- Validate authentication status on every protected request.
- Do not rely on frontend redirects or hidden navigation links for security.

## What I Learned

During this laboratory, I learned:

- How multi-factor authentication should work.
- Why successful password authentication does not necessarily mean full authentication.
- How improper session management can allow 2FA bypass.
- Why frontend navigation restrictions are insufficient.
- How server-side authorization checks protect sensitive resources.
- How to test authentication workflows systematically.

## Evidence

Screenshots are stored in the `screenshots/` directory.

- `10-2fa-verification-page.png` — Second-factor verification page.
- `10-2fa-bypass-wiener.png` — Direct account access without completing 2FA.
- `10-2fa-bypass-solved.png` — Successful bypass and solved laboratory.
