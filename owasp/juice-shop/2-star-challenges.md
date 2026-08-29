# ⭐️⭐️ 2 Star Challenges

## Challenge Name : Reflected XSS

**Category** : XSS

**Objective** : Perform a reflected XSS attack.

I went through the app testing input fields for reflected input — that's usually where reflected XSS shows up, when user-supplied data gets echoed back in the response without sanitization. Found it in the `id` parameter on the Track Orders URL.

`http://localhost:3000/#/track-result/new?id=%3Ciframe%20src%3D%22javascript:alert(%60xss%60)%22%3E`

**Key Takeaway**: URL parameters reflected into the page without sanitization are just as exploitable as form fields. Test every input source — not just what's visible on screen.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-27 at 9.42.47 PM.png" alt=""><figcaption></figcaption></figure>

***

## Challenge Name : Exposed Credentials

**Category :** Sensitive Data Exposure&#x20;

**Objective :** To find the hardcoded credentials of a testing account.

I used the web dev tools to look at the main.js file and searched for `"testing"`, which gave me the credentials that were used for testing. These credentials were still valid.

`user mail : testing@juice-sh.op password : IamUsedForTesting`

**Key Takeaways :** Sensitive information like valid credentials can end up exposed in client-side source. Searching bundled JS for keywords like "test" or "testing" is a cheap first move worth doing on any target.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-28 at 9.40.44 PM.png" alt=""><figcaption></figcaption></figure>

***

## Challenge Name : Login Admin

**Category :** Injection

**Objective :** Login with the admin user account by applying SQL injection.

I got the user email of the admin at one of the product reviews, and by using that, I tried logging in. A simple SQL payload after the user email helped me login as admin. Payload :&#x20;

`'OR 1=1 --`

**Key Takeaways :** Unsanitized SQL input is still shockingly common — a payload this basic shouldn't be enough to walk into someone's account, but it was. Parameterized queries fix this properly.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-28 at 10.45.58 PM.png" alt=""><figcaption></figcaption></figure>

***

## Challenge Name : Password Strength

**Category :** Broken Authentication

**Objective :** Login with the admin user account by using user credentials.

I already had the admin's email from an earlier challenge, so the obvious first move was just guessing the password — and a few tries in, that actually worked. For the more technical route, I intercepted the login POST request in Burp Suite and ran it through Intruder with a payload file until I landed on the right one.

`Password : admin123`

**Key Takeaways :** No rate-limiting on login attempts, so Intruder just brute-forced its way through with zero resistance. Lock the account after a few failed tries and this stops working, regardless of how weak the password is.

<div><figure><img src="../../.gitbook/assets/Screenshot 2026-08-28 at 10.47.05 PM.png" alt=""><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/Screenshot 2026-08-28 at 10.51.13 PM.png" alt=""><figcaption></figcaption></figure></div>

***
