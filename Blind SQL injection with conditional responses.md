**Lab name:Blind SQL injection with conditional responses**
<br>
*Link to Lab* https://portswigger.net/web-security/sql-injection/blind/lab-conditional-responses
<br><br><br>

### The exploit
From the labs description, we know that if the results of the SQL query are returned, a 'Welcome back' message can be seen meaning that this will show us if our SQL injection workes.
We need to access a table called 'users' with columns named 'username' and 'pasword'. For this lab, BurpSuite will be required due to the use of tracking cookies.
<br><br>Whilst using Burp suite we are able to see this code snippet when applying a filter:
``` <div>Welcome back!</div>```
To confirm that the message disspears when there is an invalid input, we can send the filter search query to the repeater tab and then append this wuery onto the cookies:
```'and 0=1--```.
When this is sent to the website, there is no 'Welcome back!' message.<br><br>
Therefore, also on the repeater tab, we can append ```' UNION SELECT NULL FROM USERS--``` with a varying number of ```NULL```'s to see if the 'users' table exists and if yes how many colums does it have..
When one ```NULL```'s were used, the welcome back message was visible meaning that there is a table called users.
Then to test if there is a user named 'administrator', we can append this query: ```' AND (SELECT 'x' FROM users WHERE username='administrator')='x```
<br><br> Next, we need to determine the length of the password by using this query: ```' AND (SELECT 'x' FROM users WHERE username='administrator' AND LENGTH(password)>1)```.
We can use the Intruder tab, placing the payload markers around the '1' and then making the payload type sequential numbers from 1-30.
<img width="510" height="272" alt="image" src="https://github.com/user-attachments/assets/7ce879d9-b7a3-4011-8c46-38d4a3d4227a" />
The 20th payload was the first to lack the Welcome back message meaning that the password is 20 characters long.
<br><br>
Next we need to determine what letters make up each position of the string.
To do this we can use this payload:```' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='x``` with the payload markers around the 'x' and '1', for this turn on the cluster bomb attack.
<img width="202" height="51" alt="image" src="https://github.com/user-attachments/assets/dfb32092-bb48-42a8-92d6-95820fc4a0f8" />
For the payload for '2', use all alphanumeric characters, done by using the Brute forcer payload type.
For the paylaod for '1', only use numbers ranging from 1-20.
When looking through the responses, only some will have a different length, these are the ones containing characters presnt in the password.
From this, I gathered my administrator's but this is different for every user.
### Why the exploit works 
The exploit uses the fact that the welcome back message only appears after a succesful query to determine whether the inputs are succesful. By doing so, we can find which values satisfy certain conditions such as length and the actual characters in the password.
This allows us to use SQL injection, even when the answers aren't shown on the screen.
