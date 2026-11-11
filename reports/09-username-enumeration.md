# Username Enumeration via Different Responses

## Environment

- Platform: PortSwigger Web Security Academy
- Lab: Username enumeration via different responses
- Difficulty: Apprentice
- Status: Solved
- Tools: Burp Suite Community Edition, Intruder, Repeater, Chrome

## Summary

The application contains an authentication vulnerability that allows username enumeration through different error messages.

The server returns different responses depending on whether a username exists.

This information can be used to identify valid accounts and facilitate password attacks.

## Steps Performed

1. Opened the PortSwigger laboratory.
2. Submitted a login attempt with an invalid username and password.
3. Observed the message `Invalid username`.
4. Intercepted the `POST /login` request using Burp Suite.
5. Sent the request to Repeater and inspected the server response.
6. Configured Burp Intruder to test candidate usernames.
7. Identified a valid username based on the `Incorrect password` response.
8. Configured another Intruder attack using the valid username and the lab's password list.
9. Compared HTTP status codes and response lengths.
10. Identified a `302` redirect indicating a potentially successful login.
11. Confirmed successful authentication in the browser.
12. Verified that the laboratory status changed to **Solved**.

## Vulnerable Behavior

For an invalid username, the application returned:

```text
Invalid username
```

For an existing username with an incorrect password, it returned:

```text
Incorrect password
```

These different responses reveal whether a specific account exists.

## Observed Results

| Test                           | HTTP Status | Response            |
| ------------------------------ | ----------- | ------------------- |
| Invalid username               | 200         | Invalid username    |
| Valid username, wrong password | 200         | Incorrect password  |
| Valid username and password    | 302         | Redirect to account |

**Final result: Lab Solved.**

## Impact

Username enumeration can help attackers identify existing accounts.

Combined with weak passwords and insufficient login protections, this may facilitate unauthorized access.

Potential consequences include:

- Account discovery
- Password guessing attacks
- Credential stuffing
- Account takeover

## Recommended Fix

- Return generic authentication error messages.
- Avoid revealing whether usernames exist.
- Implement rate limiting and automated abuse detection.
- Support multi-factor authentication.
- Enforce secure password policies.
- Monitor suspicious login activity.

Example of a safer error message:

```text
Invalid username or password.
```

## What I Learned

During this laboratory, I learned:

- How username enumeration vulnerabilities work.
- How to intercept login requests using Burp Suite.
- How to analyze authentication responses in Repeater.
- How to configure Sniper attacks in Burp Intruder.
- How to compare HTTP response status codes and lengths.
- Why authentication errors should not reveal account existence.
- How login rate limiting and MFA improve account security.

## Evidence

Screenshots are stored in the `screenshots/` directory.

- `09-auth-invalid-login.png`
- `09-auth-intruder-usernames.png`
- `09-auth-valid-username.png`
- `09-auth-intruder-passwords.png`
- `09-auth-password-results.png`
- `09-auth-solved.png`
