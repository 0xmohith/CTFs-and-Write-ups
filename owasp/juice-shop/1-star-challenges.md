# ⭐️ 1 Star Challenges

## Challenge Name : Score Board

**Category:** Web&#x20;

**Objective:** Find the hidden score board page.

Hidden routes usually still show up in the JS files even when there's no link to them in the UI, so I checked the page source first. Opened `main.js` and searched for "score" — that turned up the route the score board component uses. Just appended it to the base URL:

`http://localhost:3000/#/score-board`

**Key takeaway:** Bundled JS is a goldmine for hidden functionality. If a page isn't linked anywhere, that doesn't mean it isn't there — check the client-side code before assuming it's locked down.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-25 at 8.59.04 PM.png" alt=""><figcaption></figcaption></figure>

***

## Challenge Name : Privacy Policy

**Category:** Web&#x20;

**Objective:** Read Juice Shop's Privacy Policy.

No exploit needed here — created an account, logged in, and the page was right there.

**Key takeaway:** Not every challenge is about breaking something. Registering as a real user and poking around what that unlocks is often step one in a CTF, before you even start looking for bugs.

***

## Challenge Name : Confidential Document&#x20;

**Category:** Sensitive Data Exposure&#x20;

**Objective:** Access an unprotected confidential document.

Browsing through the site, I found a link on the "About Us" page pointing to `ftp/legal.md`. That meant there was a live `/ftp` directory. Pulled up the full listing and found more than expected — including the confidential document itself:

`http://localhost:3000/ftp/acquisitions.md`

**Key takeaway:** One exposed file in a directory is often a sign there are more. Once you find a path like `/ftp`, always check whether the whole directory is browsable — directory listings leak far more than the single file that led you there.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-25 at 10.27.03 PM.png" alt=""><figcaption></figcaption></figure>

***

## Challenge Name : Error Handling

**Category:** Security Misconfiguration&#x20;

**Objective:** Trigger an error that isn't handled gracefully or consistently.

Errors like this usually come from tampering with URL paths, parameters, or form inputs. I targeted the reviews endpoint — `GET /rest/products/1/reviews` — and used Burp Suite to send malformed values through it. That was enough to get the server to throw a `500 Internal Server Error`.

**Key takeaway:** A plain GET request can still break an app if the inputs aren't validated on the backend. And a verbose error response — stack traces especially — is a security problem in its own right, not just a bug.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-25 at 10.50.55 PM.png" alt=""><figcaption></figcaption></figure>

***

## Challenge Name : DOM XSS and Bonus DOM XSS

**Category:** XSS&#x20;

**Objective:** Execute a DOM-based XSS payload.

Started simple — dropped `<h1>Hello</h1>` into the search bar to see if input gets reflected straight into the page. It did. From there I tried two payloads: `<iframe src="javascript:alert(\`xss\`)">\`, which popped an alert, and an iframe embedding a SoundCloud player, which loaded and played audio.

**Key takeaway:** Unsanitized input reflected into the DOM is a script execution risk, full stop. A basic payload in any input field is worth trying first — it tells you fast whether there's a real problem underneath.

***

## Challenge Name : Missing Encoding

**Category:** Improper Input Validation&#x20;

**Objective:** Retrieve the photo of Bjoern's cat in "melee combat mode."

Noticed a picture on the page that wasn't loading. Dev tools showed the image source had a `#` in the filename — and browsers treat `#` as a page-section marker, so everything after it gets chopped off before the request even reaches the server. URL-encoding it as `%23` fixed the link.

**Key takeaway:** Special characters in filenames or URLs need encoding, or the browser silently drops part of the path. A broken image link is sometimes just an encoding bug, not a missing file.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-26 at 1.42.02 PM.png" alt=""><figcaption></figcaption></figure>

***

## Challenge Name : Mass Dispel

**Category:** Web&#x20;

**Objective:** Close every "Challenge solved" notification at once.

Dug through the frontend JS for anything related to closing notifications, and found a shortcut: `Shift + X` clears them all in one go.

**Key takeaway:** Source code often hides shortcuts and functions that never show up anywhere in the UI. Worth a look whenever a task feels like it should have a faster path.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-26 at 3.12.33 PM.png" alt=""><figcaption></figcaption></figure>

***

## Challenge Name : Repetitive Registration

**Category:** Improper Input Validation&#x20;

**Objective:** Register a user with password and confirm-password that don't match.

The form enforces matching passwords, but that's only a frontend rule — so I skipped it. Intercepted the `POST /api/users` request in Burp Suite and set different values for the two password fields before forwarding it.

**Key takeaway:** Client-side checks are conveniences, not security. If the backend isn't independently validating the same thing, anyone can bypass the form entirely by crafting the request directly.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-26 at 3.27.50 PM.png" alt=""><figcaption></figcaption></figure>

***

## Challenge Name : Exposed Metrics

**Category:** Observability Failures&#x20;

**Objective:** Find the endpoint serving metrics data for Prometheus to scrape.

Prometheus apps almost always expose metrics at `/metrics`, so I just tried it directly:

`http://localhost:3000/metrics`

It worked — server performance and traffic data, all sitting there unauthenticated.

**Key takeaway:** Monitoring endpoints are built for internal tooling, not the public internet. Leave one open without auth and it leaks real operational detail about your app and infrastructure.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-26 at 3.46.04 PM.png" alt=""><figcaption></figcaption></figure>

***

## Challenge Name : Outdated Allowlist

**Category:** Unvalidated Redirects&#x20;

**Objective:** Find a redirect to a cryptocurrency address that's no longer promoted.

Found a crypto address embedded in the page source and hit the redirect endpoint directly:

`./redirect?to=https://blockchain.info/address/1AbKfgvw9psQ41NbLi8kufDQTezwG8DRZm`

**Key takeaway:** Redirect allowlists rot if nobody maintains them. Removing a link from the visible site doesn't remove the underlying rule — old entries like deprecated donation addresses can stay live and exploitable indefinitely.

***

## Challenge Name : Web3 Sandbox

**Category:** Broken Access Control&#x20;

**Objective:** Find an accidentally deployed code sandbox.

Turned up a path in dev tools for a Web3 sandbox that shouldn't have been reachable:

`http://localhost:3000/#/web3-sandbox`

**Key takeaway:** Dev and test features have a way of surviving into production if nobody explicitly locks them down or rips them out. Worth checking for routes that have no business being public.

***

## Challenge Name : Zero Stars

**Category:** Improper Input Validation&#x20;

**Objective:** Submit a zero-star feedback rating.

The UI won't let you go below one star, so that's clearly a frontend-only restriction. Intercepted the feedback POST request in Burp Suite and changed the rating value to 0 before sending it through.

**Key takeaway:** Same story as the password fields above — a frontend minimum is worthless if the backend doesn't enforce it too. Anything the UI "prevents" is worth testing at the request level.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-26 at 9.08.04 PM.png" alt=""><figcaption></figcaption></figure>
