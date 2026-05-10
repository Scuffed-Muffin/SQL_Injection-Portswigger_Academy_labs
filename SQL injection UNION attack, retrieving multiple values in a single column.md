**Lab name:SQL injection UNION attack, retrieving multiple values in a single column**
<br>
*Link to Lab* https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-multiple-values-in-single-column
<br><br><br>

### The exploit
From the labs description, we knwo that there is a tavle called category with two columns named username and password. 
We also know thatwe need to retrieve all usernames and passwords so that we can log in as the administrator. 
To do this, we must concatenate the username and passwords colums using ```||```. 
To seperate the two lists, we can add a ```-``` between the two lists.
Therefore, our payload should look like this: ```' UNION SELECT username || '-' || password FROM users--```.
However, this caused an internal server error making me think that there is an extra column that is unaccounted for.

### Why the exploit works 
