# Web Security Labs

My learning notes and reports from PortSwigger Web Security Academy.

All exercises use intentionally vulnerable training environments.

## Topics practiced

- SQL injection: retrieving hidden products
- SQL injection: authentication bypass
- Reflected cross-site scripting (XSS)
- Stored cross-site scripting (XSS)
- DOM-based cross-site scripting (XSS)
- Broken access control: unprotected administrative functionality
- Insecure Direct Object References (IDOR)

## Reports

- [Unprotected admin functionality](reports/01-unprotected-admin.md)
- [SQL injection: retrieving hidden data](reports/02-sql-injection-hidden-data.md)
- [SQL injection: login bypass](reports/03-sql-injection-login-bypass.md)
- [Reflected XSS](reports/04-reflected-xss.md)
- [Stored XSS](reports/05-stored-xss.md)
- [IDOR: User profile access](reports/06-idor-user-profile.md)
- [DOM-based XSS: document.write sink](reports/07-dom-based-xss.md)
- [DOM-based XSS: innerHTML sink](reports/08-dom-xss-innerhtml.md)

## Report structure

Each report describes the environment, reproduction steps, expected and
observed behavior, impact, and recommended fixes.

## Tools used

- Browser
- Chrome DevTools
- Burp Suite Community Edition: Proxy HTTP history and Repeater
