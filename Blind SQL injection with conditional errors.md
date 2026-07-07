
**Lab name:Blind SQL injection with conditional errors**
<br>
*Link to Lab* https://portswigger.net/web-security/sql-injection/blind/lab-conditional-errors
<br><br><br>

### The exploit
Since the vulnrability relies on different error responses, initially we must identify what a normal response is and what an extraneous response is.
After this we can use these responses to find the password of the ```administrator``` user located in the ```passwords``` column of the ```users``` table.
<br>
Initially, we must establish the response of the website when there is an error, to do this we can use one of the payloads provided by the website.<img width="761" height="358" alt="image" src="https://github.com/user-attachments/assets/6ef88cee-a81d-4f96-a5d3-254f51c0b151" /> <br>By replacing the "YOUR-CONDITION-HERE" with 1=2 and appending ```' AND``` before the payload you get something like this 
```' AND SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE NULL END FROM dual```. However this payload, and similarly structured ones for different versions didn't work because an error was produced when the condition was 1=1 or 1=2. <br>
This error dissapears after removing the ```'``` making me think that it is a syntax error.
If we assume that it is an oracle database, we know that we must use the ```|| ``` in order to "stick together" the two seperate queries.<br>
Therefore we can construct this payload:
```TrackingId=abc' ||(SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE NULL END FROM dual)||' ```
<br><br> When we do this, the website only outputs an error when ```1=1``` rather than ```1=2```.
Since we have created a baseline payload, we can manipulate the condition in order to determine the password to the administrator user,
through string concatenation.
<br> Firstly, to verify that a user named administrator is present we must change one of the conditions in the payload. Therefore, I tried using this payload:
```TrackingId=abc' ||(SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE NULL END FROM users WHERE username = 'administrator')||' ```
However, this resulted in an error when it shouldn't have, making me reconsider my syntax.<br>
After some research, I found out that some oracle databases may output an error due to the lack of data type assigned to the value ```NULL```. This means that if I replace the ```NULL``` value with ```""``` , the payload should work.
<br> To find the length of the password, I tried using this payload ```' ||(SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE NULL END FROM users WHERE username = 'administrator' AND length(password) > 2)||'``` however an error always occured, no matter if the length was > 2, < 2 or = 2.
Therefore, after a quick search I found that I should place the length related condition after the ```SELECT CASE``` statment, creating this payload:```
' ||(SELECT CASE WHEN LENGTH(password)>2 THEN TO_CHAR(1/0) ELSE NULL END FROM users WHERE username = 'administrator')||'```
SInce an error occurred, I knew that the length must be greater than two. After using the repeater to use different values, I found that the passwords length is 20 characters.
<br>
In order to find out the actual characters in the password I used teh substring function, creating this payload :```
' ||(SELECT CASE WHEN SUBSTR(password,1,1) = "" THEN TO_CHAR(1/0) ELSE NULL END FROM users WHERE username = 'administrator')||'```
After sending this to the Intruder tab, I set out a cluster bomb attack where I assigned this to one payload:<img width="905" height="530" alt="image" src="https://github.com/user-attachments/assets/464b807d-d99e-4776-bbc7-b049b9f93938" /><img width="907" height="610" alt="image" src="https://github.com/user-attachments/assets/89503965-a3e1-4078-be80-41e9b44db51d" />
After letting this run...
### Why the exploit works 
