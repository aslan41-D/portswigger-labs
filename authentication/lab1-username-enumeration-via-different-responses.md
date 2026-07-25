# Username enumeration via different responses Solution 
Used Technique Attackt : BruteForce 

We will use the lists of usernames and passwords provided to us to solve the lab.
Step 1 - In Burp, go to Proxy > HTTP history and find the POST /login request. Highlight the value of the username parameter in the request and send it to Burp Intruder.

notice that the username parameter is automatically set as a payload position. This position is indicated by two § symbols, for example: username=§invalid-username§. Leave the password as any static value for now. 

Make sure that Sniper attack is selected. 

Under Payload configuration, paste the list of candidate usernames. Finally, click Start attack. The attack will start in a new window

<img width="1862" height="790" alt="Ekran Görüntüsü 2026-07-25 17-48-16" src="https://github.com/user-attachments/assets/e5293747-0a35-473f-9ee8-a4de8127ccca" />


<img width="1862" height="790" alt="Ekran Görüntüsü 2026-07-25 18-07-07" src="https://github.com/user-attachments/assets/5100686c-bb4f-4687-84b4-373fc67d9fdc" />

If you've noticed, the responses are different lengths, and they contain an incorrect password instead of an invalid username.

Now we're figuring out the password 


<img width="1862" height="790" alt="Ekran Görüntüsü 2026-07-25 18-10-59" src="https://github.com/user-attachments/assets/739dd194-e6ec-4749-8dad-07869f845168" />

Finished this lab
