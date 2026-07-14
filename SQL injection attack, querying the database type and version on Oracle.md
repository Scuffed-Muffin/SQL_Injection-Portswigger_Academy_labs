**Lab name:SQL injection attack, querying the database type and version on Oracle**
<br>
*Link to Lab* https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle
<br><br><br>

### The exploit
From the labs title and description, we know that we will need to use a UNION attack and that the databases type is Oracle.
From previous labs, we can assume that the SQL vulnrability will lie in the gifts function meaning that we will have to manipulate what we place in the browser.
To retrieve the databse version, we must use one of two payloads ```SELECT banner FROM v$version``` or ```SELECT version FROM v$instance```.
Therefore, I tried appending ```' UNION SELECT banner FROM v$version--``` after the ```category=Gifts``` query. 
However, this resulted in an Internal server error. <br>
Thinking back to previous labs, this made me think that there may be extra columns which I did not account for. 
To test the amount of columns within the ```v$version``` table. To test this, I used this payload: ```' UNION SELECT NULL,NULL FROM v$version--```.
This told me that there are two columns within the ```v$version``` table. Therefore I adjusted my initial payload, making it into  ```' UNION SELECT banner,NULL FROM v$version-- ```
This payload worked, leading the lab to be finished.

### Why the exploit works 
This ecploit works because the website designer, hasn't considered possible union vulnrabilities in the category query. This lab has also taught me to commit to labs in a slower, more methodical process, starting with the basics such as identifying the number of columns every time.
