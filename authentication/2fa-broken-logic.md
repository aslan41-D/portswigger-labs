# 2FA Broken Logic 


We bypassed it in this lab because the “verify” parameter was not properly checked.
First, we log in, then we retrieve the OTP code and view it through Burp. Next, we replace the username in the request with the target account’s username. 

After making this change, we perform a brute-force attack on the OTP code to find the correct code, and we’re logged in. 

Here, we verify our identity to ensure the system is certain it’s us.

<img width="1885" height="643" alt="Ekran Görüntüsü 2026-07-26 21-18-16" src="https://github.com/user-attachments/assets/dd57eb6b-1536-4d7b-b3aa-9d8ec92b98c3" />


<img width="1885" height="643" alt="Ekran Görüntüsü 2026-07-26 21-18-49" src="https://github.com/user-attachments/assets/b8b1cad3-9d18-4cc1-8e0a-c1b3f458ec45" />


<img width="1885" height="643" alt="Ekran Görüntüsü 2026-07-26 21-18-29" src="https://github.com/user-attachments/assets/55d4237e-46b0-4a2d-a1d5-727f53ad5bdf" />
