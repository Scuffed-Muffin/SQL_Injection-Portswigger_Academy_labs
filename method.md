**Lab name: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data**
<br>
*Link to Lab* https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data
<br><br><br>

### The exploit
When you enter the lab, you should be presented with a shopping website titled "WE LIKE TO SHOP".<br>
From the lab description, we know that when we select a categopry, the SQL query which is carried out looks like this :
```SELECT * FROM products WHERE category = 'Gifts' AND released = 1```
From the information given before the lab, Ik new that adding ```'--``` onto the back of the search query should let the website ignore the ```released``` paramater.
However, this didn't work. <br>Therefore I decided to put the other recommended exampe into the query so it looked like this:
```products?category=Gifts'+OR+1=1--```
This let me see unreleased items and the lab was marked as complete.
### Why the exploit works
Since the website uses this SQL Query to show results ```SELECT * FROM products WHERE category = 'Gifts' AND released = 1```,
by adding ```+OR+1=1``` it means that the query will return all items since either the items are part of "gifts" or 1=1. 
Additionally, the ```--```at the end of the query means that the rest of the SQL qery is turned into a comment, bypassing the effects that ```released = 1 would``` normally have.
