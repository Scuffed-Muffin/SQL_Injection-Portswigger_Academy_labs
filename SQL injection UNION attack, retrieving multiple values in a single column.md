**Lab name:SQL injection UNION attack, retrieving multiple values in a single column**
<br>
*Link to Lab* https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-multiple-values-in-single-column
<br><br><br>

### The exploit
From the labs description, we know that there is a table called category with two columns named username and password. 
We also know thatwe need to retrieve all usernames and passwords so that we can log in as the administrator. 
To do this, we must concatenate the username and passwords colums using ```||```. 
To seperate the two lists, we can add a ```-``` between the two lists.
Therefore, our payload should look like this: ```' UNION SELECT username || '-' || password FROM users--```.
However, this caused an internal server error making me think that there is an extra column that is unaccounted for.
Therfore, I statred from scratch. I appended ```' UNION SELECT NULL--``` onto the search query, adding ```NUll```'s until there was no internal server error.
This occured when two  ```NULL ```'s were presnt meaning that there are two columns in the table. 
Then I discovered which columns contained strings replacing the ```NULL``` with an ```'a'```.
There was no error when I submitted ```' UNION SELECT NULL,'a'```--.
Therefore, I altered the payload to look like: ```' UNION SELECT NULL,username || '-' || password FROM users--```
From this, I knew that the username is 'administrator' and the password is 'ws7i4q4a9er5mcuisvk1'

### Why the exploit works 
This exploit joins the usernames and passowrds together using concantation. 
As a result, we can grab the result of both in a single query which allows the string type.
The rest of how the exploit works is listed in the previous writeups.
