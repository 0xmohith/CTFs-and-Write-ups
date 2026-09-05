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

## Challenge Name : Admin Section&#x20;

**Category :** Broken Access Control

**Objective :** To access the administration section of the store.

Just like the "score board" challenge, I searched for the path for the admin section, and it was openly available in the main.js file, without any authorization checks.

`http://localhost:3000/#/administration`

**Key Takeaways :** Client-side Java Script isn't a security boundary. If the server doesn't check who's asking, anyone who reads the source code gets in.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-28 at 11.18.23 PM.png" alt=""><figcaption></figcaption></figure>

***

## Challenge Name : Five-Star Feedback

**Category :** Broken Access Control

**Objective :** To get rid of all 5-star feedbacks.

Solving the Admin Section challenge hands you admin privileges, which meant I could see every 5-star review and just delete them.

`http://localhost:3000/#/administration`

**Key Takeaways :** No proper auth checks, can lead to severe security breaches, allowing unauthorized users to compromise systems and data. Implementing a multi-layered security strategy across the codebase could prevent it.

***

## Challenge Name : View Basket

**Category :** Broken Access Control

**Objective :** To view another user's shopping basket.

Intercepting the GET requests to view one's own basket, revealed the user id in the request line itself. Just by changing the user id, I was able to view someone else's shopping basket.

**Key Takeaways :** It is a classic example of IDOR (Insecure Direct Object Reference), which means the application is relying entirely on the client-side input to determine access rights, rather than validating. Writing Contextual Access Control Checks would prevent it.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-28 at 11.22.50 PM.png" alt=""><figcaption></figcaption></figure>

***

## Challenge Name : Deprecated Interface

**Category :** Security Misconfiguration

**Objective :** To use a deprecated b2b interface that was not properly shut down.

A B2B interface is a digital connection that lets two companies share data, and one of the common technologies used is 'File Uploads'. Upon inspection of the source code, I discovered that the B2B interface allows uploads of `.xml` files. Then, I used the complaint interface to upload a `.xml` file.

**Key Takeaways :** Because these interfaces handle sensitive data, leaving old or unused B2B upload forms active can create a major security risk. They must be shut down properly, by completely removing the legacy code.

<figure><img src="../../.gitbook/assets/Screenshot 2026-09-01 at 12.25.05 PM.png" alt=""><figcaption></figcaption></figure>

***

## Challenge Name : Login Mc SafeSearch

**Category :** Sensitive Data Exposure

**Objective :** To log in with MC SafeSearch's original user credentials without applying SQL Injection or any other bypass.

After going through all the product reviews, I got the mail id of Mc SafeSearch, a rapper. In his famous work "Protect Ya Passwordz", he reveals the password to his account. Upon closer inspection to the lyrics and some changes, I got the correct password to his account.

`Mr. N00dels`

**Key Takeaways :** The sensitive data exposure vulnerability occurs when sensitive information like user credentials are disclosed publicly, on social medias and other platforms. This is a classic text-book example of attacks often starting with basic reconnaissance. &#x20;

***

## Challenge Name : Empty user Registration

**Category :** Improper Input Validation

**Objective :** To register a user with empty email and password.

Trying to register an empty user on the browser is not possible because of browser UI restrictions. Intercepting the "`POST Users`" request through BurpSuite, and modifying the credentials allowed me to register an empty user. After registration, it can be verified by logging in the same way.

**Key Takeaways :** The Juice Shop frontend uses HTML5 attributes (like `required`) and JavaScript to prevent a user from clicking "Register" if the fields are empty. Relying solely on browser to validate input is a massive vulnerability. There must be a strict database constraints or schema integrity.

<div><figure><img src="../../.gitbook/assets/Screenshot 2026-09-05 at 2.43.32 PM.png" alt=""><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/Screenshot 2026-09-05 at 2.45.43 PM.png" alt=""><figcaption></figcaption></figure></div>

***

## Challenge Name : Meta Geo Stalking

**Category :** Sensitive Data Exposure

**Objective :** To determine John's security question's answer by looking at an upload of him at photo wall.

The 'Forgot Password' mechanism for `john@juice-sh.op` reveals his security question - "What is your favorite place to visit". Following the clue, I downloaded and used `exiftool` on the image uploaded by John, to get the GPS position which revealed his favorite place :&#x20;

`Daniel Boone National Forest`

**Key Takeaways :** When a photo is taken on a smartphone, data like location is also embedded in a hidden chunk of metadata called EXIF. The vulnerability exists because the application accepts a file from a user and serves it back to the public exactly as it was received.

<figure><img src="../../.gitbook/assets/Screenshot 2026-09-05 at 3.48.55 PM.png" alt=""><figcaption></figcaption></figure>

***

