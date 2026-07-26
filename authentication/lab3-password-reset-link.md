# Password reset broken logic


We log in using the account information he provided us, then change the password, and see that the transaction was made via a token.



In this lab, it doesn't check which user the token belongs to; most likely, the code in the background simply checks whether a token exists 
and whether the email address of the token's owner is accessible, or something like this => 




if token_is_valid(temp_token):


   reset_password(username)

    

<img width="1544" height="714" alt="Ekran Görüntüsü 2026-07-26 19-57-25" src="https://github.com/user-attachments/assets/5d63c0c3-c7bc-47ee-98d4-f3d6130bc308" />

<img width="1544" height="714" alt="Ekran Görüntüsü 2026-07-26 19-57-32" src="https://github.com/user-attachments/assets/b3a1e203-c71f-4f0a-8c91-8403859e51c3" />
