# DOM-based Cross-Site Scripting via `document.write()`

## Environment

- Platform: PortSwigger Web Security Academy
- Lab: DOM XSS in document.write sink using source location.search
- Difficulty: Apprentice
- Status: Solved
- Tools: Google Chrome, Chrome DevTools

## Summary

The application contains a DOM-based Cross-Site Scripting (XSS) vulnerability.

The search functionality reads user-controlled input from the URL using `window.location.search` and passes it directly to `document.write()` without proper sanitization.

This allows an attacker to inject HTML and execute JavaScript in the browser.

## Steps Performed

1. Opened the PortSwigger laboratory.
2. Entered `test123` into the search field.
3. Confirmed that the URL contained `?search=test123`.
4. Opened Chrome DevTools and inspected the JavaScript code.
5. Identified the vulnerable `document.write()` function.
6. Tested HTML injection using:

   `"><h1>TEST123</h1>`

7. Confirmed that the browser rendered the injected heading.
8. Tested DOM-based XSS using:

   `"><svg onload=alert(1)>`

9. Observed a JavaScript alert displaying `1`.
10. Confirmed that the laboratory status changed to **Solved**.

## Vulnerable Code

```javascript
function trackSearch(query) {
  document.write(
    '<img src="/resources/images/tracker.gif?searchTerms=' + query + '">',
  );
}

var query = new URLSearchParams(window.location.search).get("search");

if (query) {
  trackSearch(query);
}
```

**Source:** `window.location.search`

**Sink:** `document.write()`

**Vulnerability:** DOM-based XSS

## Expected Behavior

User input should be processed as plain text and must not be interpreted as executable HTML or JavaScript.

## Observed Behavior

The application interpreted user-controlled HTML and executed JavaScript through an injected SVG element.

The `alert(1)` dialog appeared, confirming successful JavaScript execution.

**Final result: Lab Solved.**

## Impact

A similar vulnerability in a real application could allow attackers to execute JavaScript in another user's browser.

Potential consequences include:

- Unauthorized actions within the application
- Access to sensitive page information
- Manipulation of displayed content
- Phishing attacks

## Recommended Fix

Avoid using `document.write()` with untrusted user input.

Use safe DOM manipulation methods such as:

```javascript
const query = new URLSearchParams(window.location.search).get("search");

if (query) {
  const img = document.createElement("img");

  const url = new URL("/resources/images/tracker.gif", window.location.origin);

  url.searchParams.set("searchTerms", query);

  img.src = url.toString();
  document.body.appendChild(img);
}
```

Additional security recommendations:

- Validate untrusted input.
- Use `textContent` for text output.
- Avoid unsafe HTML injection.
- Implement a restrictive Content Security Policy.
- Apply context-appropriate output encoding.

## What I Learned

During this laboratory, I learned:

- How DOM-based XSS vulnerabilities occur.
- The difference between a JavaScript source and sink.
- How to inspect JavaScript using Chrome DevTools.
- How HTML injection can lead to JavaScript execution.
- How to prevent DOM-based XSS using safe DOM APIs.

## Evidence

- `07-dom-xss-source.png` — Vulnerable JavaScript code
- `07-dom-xss-html-injection.png` — HTML injection result
- `07-dom-xss-alert.png` — JavaScript execution
- `07-dom-xss-solved.png` — Successfully completed laboratory
