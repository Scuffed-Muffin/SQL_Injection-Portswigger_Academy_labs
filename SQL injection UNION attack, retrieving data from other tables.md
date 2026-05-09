**Lab name:SQL injection UNION attack, retrieving data from other tables**
<br>
*Link to Lab* https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-data-from-other-tables
<br><br><br>

### The exploit
From the labs description, we know that we need to use a UNION attack that retrieves all ```usernames``` and ```passwords``` from the ```users``` database.
Since we know what the columns are named and the name of the database, we can use this payload: ```' UNION SELECT username, password FROM users--```.
This shows us multiple names and passwords beneath each items' description.
This let us see that the the username is: administrator ,and the password is: 6h70i9xt43avwnsydvxv.
So then, we can go onto My account and input the details given.
### Why the exploit works
We were able to skip the steps of identifying how many columns there are and which can hold strings due to the labs description.
This meant that we could skip straight to payload creation, and answer the lab quickly.
