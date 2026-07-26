# 2FA simple bypass Lab

Used tech : Change to URL :D

First, we log in using the credentials provided to us: wiener:peter. Then we enter the account, and a 4-digit OTP verification code is sent.

We retrieve it from the email client on the same page and paste it in. 


Second, we use the other credentials provided to us to log in to the carlos:montoya account.

The system then prompts us for a verification code, but instead of entering the code, we bypass the login section by deleting the “login” part of the URL and replacing it with “/my-account.” This allows us to send a request via the URL without the system checking whether the code was entered correctly or not. 

<img width="577" height="378" alt="image" src="https://github.com/user-attachments/assets/56b14f11-8573-4151-8261-1f61b0d5574c" />
