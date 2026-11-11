# DOM-based XSS via innerHTML

## Environment

- Platform: PortSwigger Web Security Academy
- Lab: DOM XSS in innerHTML sink using source location.search
- Difficulty: Apprentice
- Status: Solved
- Tools: Google Chrome, Chrome DevTools

## Summary

The application contains a DOM-based Cross-Site Scripting (XSS) vulnerability in its search functionality.

User-controlled input from the URL is assigned directly to the `innerHTML` property without sanitization.

This allows injected HTML elements and JavaScript event handlers to be executed in the browser.

## Steps Performed

1. Opened the PortSwigger laboratory.
2. Entered `test123` into the search field.
3. Confirmed the search parameter in the URL.
4. Inspected the application using Chrome DevTools.
5. Identified the vulnerable `innerHTML` assignment.
6. Tested HTML injection with `<h1>TEST123</h1>`.
7. Confirmed that the injected heading was rendered.
8. Tested DOM XSS with `<img src=x onerror=alert(1)>`.
9. Observed the JavaScript alert displaying `1`.
10. Confirmed the laboratory status changed to **Solved**.

## Vulnerable Code

```javascript
function doSearchQuery(query) {
  document.getElementById("searchMessage").innerHTML = query;
}

var query = new URLSearchParams(window.location.search).get("search");

if (query) {
  doSearchQuery(query);
}
```

**Source:** `window.location.search`

**Sink:** `innerHTML`

**Vulnerability:** DOM-based Cross-Site Scripting

## Expected Behavior

User input should be displayed as plain text rather than interpreted as HTML.

## Observed Behavior

The application interpreted user-controlled input as HTML.

The injected image element triggered an error event, executing JavaScript through its `onerror` handler.

The `alert(1)` dialog appeared, confirming successful JavaScript execution.

**Final result: Lab Solved.**

## Impact

In a real-world application, DOM-based XSS could allow an attacker to:

- Execute JavaScript in a victim's browser.
- Manipulate page content.
- Perform unauthorized actions in the user's session.
- Access sensitive information available to JavaScript.
- Create phishing interfaces.

## Recommended Fix

Avoid assigning untrusted input directly to `innerHTML`.

Use `textContent` when displaying plain text:

```javascript
function doSearchQuery(query) {
  document.getElementById("searchMessage").textContent = query;
}
```

Additional recommendations:

- Validate untrusted input.
- Apply context-appropriate output encoding.
- Use a trusted HTML sanitizer if HTML rendering is required.
- Implement a restrictive Content Security Policy.
- Avoid unsafe DOM sinks.

## What I Learned

During this laboratory, I learned:

- How DOM-based XSS vulnerabilities occur through `innerHTML`.
- How URL parameters can become dangerous input sources.
- How to identify vulnerable JavaScript using Chrome DevTools.
- Why HTML event handlers can execute JavaScript.
- Why scripts inserted via `innerHTML` generally do not execute.
- How to prevent DOM-based XSS with safe DOM APIs.

## Evidence

- `08-innerhtml-source.png` — Vulnerable JavaScript code
- `08-innerhtml-html-injection.png` — HTML injection result
- `08-innerhtml-alert.png` — JavaScript execution
- `08-innerhtml-solved.png` — Successfully completed laboratory
