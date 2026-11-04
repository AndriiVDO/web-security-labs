# Stored Cross-Site Scripting

## Environment

PortSwigger Web Security Academy — intentionally vulnerable training lab.

Lab: https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded

## Summary

The application stored a comment and included it in the post's HTML
without appropriate output encoding.

## Observed result

After submitting the test comment and returning to the article,
a harmless JavaScript alert executed in my browser.

The article URL contained only its post ID, without the test code.

## Expected behavior

Comments intended as plain text should be displayed as text,
without executing HTML or JavaScript.

## Impact

Visitors viewing the affected article could execute attacker-supplied
JavaScript in their browsers.

Only a harmless alert was demonstrated in this exercise.

## Recommended fix

- Apply HTML output encoding when displaying plain-text comments.
- If HTML formatting is supported, use a maintained HTML sanitizer
  with an explicit allowlist.
- Use safe DOM APIs such as textContent for plain text.
- Use Content Security Policy as an additional defense.

## What I learned

Stored XSS persists in application content.
Unlike reflected XSS, it does not require the test code
to be included in each visitor's URL.
