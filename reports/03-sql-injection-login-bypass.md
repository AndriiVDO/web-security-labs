# SQL Injection: Login Bypass

## Environment

PortSwigger Web Security Academy — intentionally vulnerable training lab.

Lab: https://portswigger.net/web-security/sql-injection/lab-login-bypass

## Summary

The login function handled user input unsafely when building a database query.

## Observed result

I logged in as the laboratory administrator without knowing the correct password.
The application displayed the administrator account and marked the lab as solved.

## Impact

An attacker could gain access to an existing account without its password.

## Recommended fix

- Use parameterized database queries.
- Retrieve the account safely and verify its password hash.
- Create an authenticated session only after successful password verification.

## What I learned

The password was neither recovered nor changed.
The vulnerability allowed its verification to be bypassed.
