This challenge has **login form**, so the first thing came up in my mind was SQL Injection at login form.  After making some checking and observation, I came up with two things: 
1. The password field is vulnerable to SQLi when I put ``1'`` into the field. The website returns SQLite3 error.
2. The injection `1' or '1'='1';` returned information and the highlight is in the flag part is which 'N/A' .Hence it can be guessed that I could find flag through dumping the table. 

The next step is finding information of that tables and its contents. 
- ``UNION SELECT`` helps figure out how many columns there are in the current table.  The syntax of the SQL statement is ``1' UNION SELECT 1;`` to check whether the table has one columns. The result shows the warning indicating that the column number is not 1. So I kept adding more numbers to check columns' number. Finally, I found that the number is 8 with input ``1' union select 1,2,3,4,5,6,7,8;``and the website returns 
-  Next, I found tables' names in the database by injecting ` 1' union select 1,2,3,4,5,6,name,8 from sqlite_master where type='table';` Since the result returned only one table, I used `limit` to read the below record. The full input is ` 3' union select 1,2,3,4,5,6,name,8 from sqlite_master where type='table' limit 1,1;`
- Finally, I found the columns names in the users table with `` 1' union select 1,2,3,4,5,6,sql,8 from sqlite_master WHERE name='users';``  This injection s for retrieving the **CREATE TABLE** statement. 

The last step is getting the information of the flag with input `1' union select username,password,avatar,age,name,title,flag,id from users where flag like 'sun%';` Since the flag starts with 'sun', I used **LIKE** to retrieving the flag quickly. 


