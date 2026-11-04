# Reflected Cross-Site Scripting

## Environment

PortSwigger Web Security Academy — intentionally vulnerable training lab.

Lab: https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded

## Summary

The search function included user input in its HTML response without
appropriate output encoding.

## Observed result

A harmless JavaScript alert executed in my browser after submitting
the test input through the search form.

The laboratory displayed the Solved status.

## Expected behavior

The application should display the search term as text,
without interpreting it as HTML or JavaScript.

## Impact

A crafted link could cause JavaScript to execute in the browser
of a user who opens it.

Only a harmless alert was demonstrated in this exercise.

## Recommended fix

- Apply output encoding appropriate to the HTML context.
- Use textContent when inserting plain text through JavaScript.
- Avoid inserting untrusted input through innerHTML.
- Use Content Security Policy as an additional defense.

## What I learned

Reflected input comes from the current request.
The injected JavaScript executes separately in each visitor's browser.
