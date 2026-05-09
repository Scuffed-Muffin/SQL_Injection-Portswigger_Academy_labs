**Lab name:SQL injection UNION attack, finding a column containing text**
<br>
*Link to Lab* https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text
<br><br><br>

### The exploit
From the lab description, we know that there is a SQL injection vulnrability in the category filter.
First, we have to find the amount of columns, then we have to find which columns use string data.
To find the amount of columns, we use ```' UNION SELECT NULL--``` adding more NULL's until a result is different.
The payload which trigegred this is ```' UNION SELECT NULL,NULL,NULL--```. 
To find which columns can contain strings, we replace the ```NULL``` with an ```'a'```, and then we do trial and error.
There was no error when the 'a' was within the second column, making the query:```' UNION SELECT NULL,'a',NULL--```.
Therefore, to solve the lab, I inputted: ```' UNION SELECT NULL,'fjvISO',NULL--```and the lab was marked as completed.
### Why the exploit works 
The method to find the amount of columns is detailed in the previous lab write-up.
By inputting an ```'a'``` instead of ```NULL```, it tells us what column is compatible with a string data type.
Therefore, since the lab required the output fjvISO, replacing the ```'a'``` with ```'fjvISO'``` would solve the lab.
