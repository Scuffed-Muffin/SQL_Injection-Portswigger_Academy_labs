**Lab name:SQL injection attack, querying the database type and version on MySQL and Microsoft**
<br>
*Link to Lab* https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-mysql-microsoft
<br><br><br>

### The exploit
Learning from the previous lab, I knew that first, I should establish the amount of columns within the database, using  ```category=Gifts``` as an enterance point. To do this I used this as a query ```'+UNION+SELECT+NULL#``` and then ```'+UNION+SELECT+NULL,NULL#```. <br>
Since the second payload resulted with no error, we know that there are two columns, therefore we replaced the ```NULL``` with quotation marks, showing which columns can output text.  Since both columns can output text I replaced one of the ```NULL``` values with ```@@version```
This marked the lab as complete since the database version string was outputted in the paragraph.
