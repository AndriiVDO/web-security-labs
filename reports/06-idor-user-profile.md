\# IDOR: User profile accessible through a request parameter



\## Environment

PortSwigger Web Security Academy — intentionally vulnerable training lab.



Lab: https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter



Tools: Burp Suite Community Edition, built-in browser, HTTP history, Repeater.



\## Summary

An authenticated user could access another user's profile by changing

the id query parameter. The server did not enforce authorization

for the requested account.



\## Steps performed

1\. Logged in with the supplied training account.

2\. Opened my account page and located its request in HTTP history.

3\. Sent the request to Repeater and confirmed the baseline response.

4\. Changed only the id parameter to the target user specified by the lab,

&#x20;  keeping the same session cookie.

5\. Retrieved the target user's API key from the response.

6\. Submitted the key through the lab's Submit solution form.



\## Expected behavior

The server should return profile data only when the authenticated user

is authorized to access that profile.



\## Observed behavior

The modified request returned HTTP 200 and exposed another user's

profile data and API key. The lab confirmed successful completion.



\## Impact

The demonstrated issue allowed unauthorized disclosure of another

user's API key. No additional use of that key was tested.



\## Recommended fix

\- Enforce authorization for every requested account.

\- For a personal profile endpoint, derive the account from the

&#x20; authenticated session when appropriate.

\- Deny access before returning sensitive data.

\- Add tests that verify isolation between different users.



\## What I learned

Authentication identifies the current user.

Authorization determines which resources that user may access.

Changing a resource identifier must never bypass authorization.



\## Evidence handling

Session cookies and API key values are omitted from this report.
