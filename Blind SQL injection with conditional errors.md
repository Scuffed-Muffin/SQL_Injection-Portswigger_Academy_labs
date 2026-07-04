
**Lab name:Blind SQL injection with conditional errors**
<br>
*Link to Lab* https://portswigger.net/web-security/sql-injection/blind/lab-conditional-errors
<br><br><br>

### The exploit
Since the vulnrability relies on different error responses, initially we must identify what a normal response is and what an extraneous response is.
After this we can use these responses to find the password of the ```administrator``` user located in the ```passwords``` column of the ```users``` table.
<br>
Initially, we must establish the response of the website when there is an error, to do this we can use one of the payloads provided by the website.<img width="761" height="358" alt="image" src="https://github.com/user-attachments/assets/6ef88cee-a81d-4f96-a5d3-254f51c0b151" /> By replacing the "YOUR-CONDITION-HERE" with 1=2 and appending ```' AND``` before the payload you get something like this 
```' AND SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE NULL END FROM dual```. However this payload, and similarly structured ones for different versions didn't work because an error was produced when the condition was 1=1 or 1=2. <br>
This error dissapears after removing the ```'``` making me think that it is a syntax error


### Why the exploit works 
