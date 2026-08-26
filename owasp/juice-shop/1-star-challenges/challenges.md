# Challenges :

## Challenge Name : Score Board

Category : Web

Objective : Locate the hidden score board page on the application.

Approach : Inspected the page source to look for client-side hints, since hidden routes are often referenced in JS files rather than exposed through the UI.

Exploitation : Opened main.js from the page source and searched (Ctrl+F) for the keyword "score". This revealed the route used to render the score board component. Appended the route to the base URL:

`http://localhost:3000/#/score-board`

Result : Successfully accessed the hidden score board page.

Key Takeaways : Client-side routes and hidden pages are often still referenced in bundled JS files, even if not linked in the UI. Reviewing source/network files is a reliable first step for discovering hidden functionality.

<figure><img src="../../../.gitbook/assets/Screenshot 2026-08-25 at 8.59.04 PM.png" alt=""><figcaption></figcaption></figure>

## Challenge Name : Privacy Policy

Category : Web

Objective : Read the Privacy Policy of Juice Shop

Approach : Created a user account, and by logging in to the account, one is easily able to read the privacy policy.

Result : Successfully read the Privacy Policy.

Key Takeaways : Not every challenge requires exploitation. Registering and logging in as a legitimate user is often the first step in a CTF, as it unlocks account-specific pages and features not accessible anonymously.

## Challenge Name : Confidential Document&#x20;

Category : Sensitive Data Exposure

Objective : Access and view an unprotected confidential document.

Approach : Explored the webpage by browsing through its different sections and links.

Exploitation : On the 'About Us' page, a link redirected to ftp/legal.md, revealing an accessible /ftp directory. Browsing its full listing exposed additional files, including the confidential document:

`http://localhost:3000/ftp/acquisitions.md`

Result : Successfully accessed and viewed the confidential document.

Key Takeaways : Directory listing on exposed paths (like /ftp) can leak sensitive files never meant for public access. Once a base path is discovered, always browse its full listing — a single accessible file often points to a whole directory of unprotected data.

<figure><img src="../../../.gitbook/assets/Screenshot 2026-08-25 at 10.27.03 PM.png" alt=""><figcaption></figcaption></figure>

## Challenge Name : Error Handling

Category : Security Misconfiguration

Objective : Provoke an error that is neither very gracefully nor consistently handled.

Approach : The error can be triggered for various conditions like tampering with URL paths or parameters, and submitting bad inputs to forms.

Exploitation : Targeted the GET endpoint `http://localhost:3000/rest/products/1/reviews`, which fetches product reviews. Using Burp Suite, modified the URL/parameter values to send unexpected input, which caused the server to respond with a `500 Internal Server Error`.

Result : Successfully provoked a 500 Internal Server Error by manipulating the URL path/parameter.

Key Takeaways : Even simple GET requests can break an app if inputs aren't validated properly. Verbose error messages (like stack traces) can leak internal details, making them a security risk on their own.

<figure><img src="../../../.gitbook/assets/Screenshot 2026-08-25 at 10.50.55 PM.png" alt=""><figcaption></figcaption></figure>

## Challenge Name : DOM XSS and Bonus DOM XSS

Category : XSS

Objective : To perform a DOM XSS attack with a suitable payload.

Approach : Tested if the search bar reflects input directly into the page using a simple payload like `<h1>Hello</h1>`.

Exploitation : Entered the payload ``<iframe src="javascript:alert(`xss`)">`` and `<iframe width="100%" height="166" scrolling="no" frameborder="no" allow="autoplay" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/771984076&color=%23ff5500&auto_play=true&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true"></iframe>` which gave an alert message and a sound player respectively, into the search bar.

Result : Successfully exploited a DOM XSS vulnerability.

Key Takeaways : User input reflected directly into the page without sanitization can execute malicious scripts. Always test input fields with simple payloads first.

## Challenge Name : Missing Encoding

Category : Improper Input Validation

Objective : Retrieve the photo of Bjoern's cat in "melee combat-mode".

Approach : Looked at the webpage to locate unloaded pictures.

Exploitation : Found an unloaded picture and used browser dev tools to inspect its image source. The filename contained a `#` symbol, which browsers treat as a page section marker and ignore everything after it — so the server only received a partial filename. Encoding the `#` as `%23` fixed the link.

Result : Successfully retrieved the photo.

Key Takeaways : Special characters like `#` in a filename need to be URL-encoded (`%23`), or the browser will cut off everything after it — causing the link to break.

<figure><img src="../../../.gitbook/assets/Screenshot 2026-08-26 at 1.42.02 PM.png" alt=""><figcaption></figcaption></figure>

## Challenge Name : Mass Dispel

Category : Web

Objective : Close multiple "Challenge solved"-notifications in one go.

Approach : Use the browser dev tools.

Exploitation : Looked through the frontend JavaScript for functions related to closing notifications. Found that pressing `Shift + X` closes all open notifications at once, and used this shortcut.

Result : Successfully closed all notifications.

Key Takeaways : Checking the source code can reveal hidden shortcuts or functions not shown anywhere in the UI.

<figure><img src="../../../.gitbook/assets/Screenshot 2026-08-26 at 3.12.33 PM.png" alt=""><figcaption></figcaption></figure>

## Challenge Name : Repetitive Registration

Category : Improper Input Validation

Objective : Register a user with improper credentials (password and confirm-password not matching).

Approach : Followed the DRY principle — instead of relying on the frontend form to enforce matching passwords, sent the registration request directly to the backend.

Exploitation : Intercepted the `POST` request to `http://localhost:3000/api/users` using Burp Suite and submitted different values for the password and confirm-password fields, bypassing the client-side check.

Result : Successfully registered a user despite mismatched password fields.

Key Takeaways : Client-side validation alone isn't enough — the backend must independently validate input, since requests can be crafted and sent directly, bypassing the frontend entirely.

<figure><img src="../../../.gitbook/assets/Screenshot 2026-08-26 at 3.27.50 PM.png" alt=""><figcaption></figcaption></figure>

## Challenge Name : Exposed Metrics

Category : Observability Failures

Objective : Find the endpoint that serves usage data to be scraped by prometheus.

Approach : Prometheus documentation states that applications typically expose metrics at the `/metrics` endpoint, so this was checked directly.

Exploitation : Appended `/metrics` to the base URL:

`http://localhost:3000/metrics`

Result : Successfully exposed metrics data, including server performance and traffic rates.

Key Takeaways : Monitoring endpoints like `/metrics` are meant for internal tools, not public access. If left unauthenticated, they can leak sensitive operational data about the application and server.

<figure><img src="../../../.gitbook/assets/Screenshot 2026-08-26 at 3.46.04 PM.png" alt=""><figcaption></figcaption></figure>

## Challenge Name : Outdated Allowlist

Category : Unvalidated Redirects

Objective : Find redirects to crypto currency addresses which are not promoted any longer.

Approach : Use the browser dev tools.

Exploitation : Found a cryptocurrency address link embedded in the source and used the redirect endpoint to access it:

`./redirect?to=https://blockchain.info/address/1AbKfgvw9psQ41NbLi8kufDQTezwG8DRZm`

Result : Successfully found redirects to outdated crypto currency addresses.

Key Takeaways : Redirect allowlists need regular maintenance — old entries (like deprecated donation/crypto addresses) can remain valid and exploitable long after they're no longer actively promoted, since removing the visible link doesn't remove the underlying redirect rule.

## Challenge Name : Web3 Sandbox

Category : Broken Access Control

Objective : Find an accidentally deployed code sandbox

Approach : Use the browser dev tools.

Exploitation : Found a path to the deployed Web3 Sandbox.

`http://localhost:3000/#/web3-sandbox`

Result : Successfully accessed the deployed sandbox.

Key Takeaways : Development or testing features (like sandboxes) can accidentally make it into production if not properly removed or access-restricted. Always check for routes that shouldn't be publicly reachable.

## Challenge Name : Zero Stars

Category : Improper Input Validation

Objective : Give a devastating zero-star feedback to the store.

Approach : Used Burp Suite to intercept the POST request generated when submitting feedback, since the UI likely restricts ratings to a minimum of 1 star.

Exploitation : Intercepted the feedback submission request and modified the rating value to 0 before forwarding it.

Result : Successfully submitted a zero-star rating.

Key Takeaways : Frontend restrictions (like minimum rating values) are just UI conveniences — if the backend doesn't independently validate the same rules, they can be bypassed entirely by intercepting and modifying the request.

<figure><img src="../../../.gitbook/assets/Screenshot 2026-08-26 at 9.08.04 PM.png" alt=""><figcaption></figcaption></figure>
