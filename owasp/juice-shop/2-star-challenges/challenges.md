# Challenges :

## Challenge Name : Reflected XSS

Category : XSS

Objective : Perform a reflected XSS attack.

Approach : Tested various input fields across the application for reflected input, since reflected XSS typically occurs when user-supplied data is echoed back in the response without proper sanitization.

Exploitation : Found that the `id` parameter in the Track Orders URL was vulnerable to reflected XSS. Injected the following payload:

`http://localhost:3000/#/track-result/new?id=%3Ciframe%20src%3D%22javascript:alert(%60xss%60)%22%3E`

Result : Successfully performed a reflected XSS attack.

Key Takeaways : URL parameters reflected directly into the page without sanitization can execute malicious scripts. Every input source — including URL parameters, not just visible form fields — should be tested for reflected XSS.

<figure><img src="../../../.gitbook/assets/Screenshot 2026-08-27 at 9.42.47 PM.png" alt=""><figcaption></figcaption></figure>

***

## Challenge Name : Reflected XSS

Category : XSS

Objective : Perform a reflected XSS attack.

Approach : Tested various input fields across the application for reflected input, since reflected XSS typically occurs when user-supplied data is echoed back in the response without proper sanitization.

Exploitation : Found that the `id` parameter in the Track Orders URL was vulnerable to reflected XSS. Injected the following payload:

`http://localhost:3000/#/track-result/new?id=%3Ciframe%20src%3D%22javascript:alert(%60xss%60)%22%3E`

Result : Successfully performed a reflected XSS attack.

Key Takeaways : URL parameters reflected directly into the page without sanitization can execute malicious scripts. Every input source — including URL parameters, not just visible form fields — should be tested for reflected XSS.
