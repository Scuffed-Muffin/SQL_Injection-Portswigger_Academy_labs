**Lab name:SQL injection UNION attack, determining the number of columns returned by the query**
<br>
*Link to Lab* https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns
<br><br><br>

### The exploit
Knowing that I had to determine the amount of columns in the database, I decided to use the NULL method.
To do this, I started by appending ```' UNION SELECT NULL--``` onto the search query.
Since this returned a server error, I increased the amount of null columns in the query.
I continued doing this until no server error occurred. The payload which did this was``` ' UNION SELECT NULL,NULL,NULL--```
This means that the database has three columns.
### Why the exploit works
This checks for the amount of Nulls in the database. 
The Null calue is convertible to every common data type meaning that it maximises the cahnces that out payload is successful. 
When the website provides a different response to normal, we know that we have gathered enough intel to tell us how many columns there are.

