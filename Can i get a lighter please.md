# Can i get a lighter please ? ( Misc ) 

- Checking the file in the HexEditor, so now we know more about this file.
> An SQLITE DATABASE ( with .db extention ).
- I opened the database using sqlite3.
```
C:\>sqlite3
sqlite> .open can_i_get_a_lighter.db
sqlite> select * from sqlite_master;
table|zip|zip|0|CREATE VIRTUAL TABLE zip USING zipfile('can_i_get_a_lighter.db')
sqlite>
```
- Hummm what the zip ? , i did some research about it.
> I just realised that our sqlite_db header is not correct :/. 
 
 ![alt text](https://i.imgur.com/Y1SpwkM.png "piwpiw") 
 
 > A valid SQLite database file begins with the following 16 bytes (in hex): 53 51 4c 69 74 65 20 66 6f 72 6d 61 74 20 33 00.
 > This byte sequence corresponds to strings "SQLite format 3"
 > including the nul terminator character at the end.
 > ~ From : https://www.sqlite.org/fileformat.html
 
 - After fixing the db file, i opened it and the result was good.
 ```
 sqlite> select * from sqlite_master;

table|intro|intro|2|CREATE TABLE intro ( x int primary key, Q text , xQx longBlob )
index|sqlite_autoindex_intro_1|intro|3|
table|party|party|4|CREATE TABLE party ( x int primary key, Q text , xQx longBlob )
index|sqlite_autoindex_party_1|party|5|
table|flag|flag|6|CREATE TABLE flag ( x int primary key, Q text , xQx longBlob )
index|sqlite_autoindex_flag_1|flag|7|
table|test_1|test_1|11|CREATE TABLE test_1 ( x int, xx text , xxx longblob)
 ```
 
 > I'm pretty sure that our flag is stored somewhere in a column type of __LONGBLOB__ 
 > Cuz BLOB is a binary large object.
 
```
Example : INSERT INTO table_name (column) VALUES (x'01DE2100...etc');  
 ```
 - I just wrote a python script to extract data from blob columns :D.
 - I saw a png file header , so the data you need to extract is a picture
 
 ```
#!/usr/bin/python
from random import *
import sqlite3 as hell
import os
connection = hell.connect('C:\can_i_get_a_lighter_please.db') # connection to our sqlite db.
extraction = 'xfiles/'                                        # folder to extract data
curs = connection.cursor()                                    # cursor
if not os.path.isdir(extraction):
	os.makedirs(extraction)
v=0
for xcolumn in curs.execute("select * from party;"):
        v+=1
#### Note :  xcolumn[1] column of shitty strings (type TEXT) || xcolumn[2] column of xQx(type BLOB) ( flag stored here ) ####
#### I used random numbers for file name ####
	with open(extraction + '/' + str(randint(666, 999))+str(randint(1, 9))+'.png', "wb") as flags:
		flags.write(x[2])
curs.close()
connection.close()
print(' Success | Extracted ' + str(v) + ' Files ')
 ```
 
 ```
 C:\>sqlighter.py
 Success | Extracted 31 Files
 ```
 - Great our flag is here !   
 ![alt text](https://i.imgur.com/VBmDO5f.png "piwpiw") 
 
 #### I hope u enjoy ####

> Greetz to all my dudes : Th3 Jackers , Ghosty , Aymen , Tr0jan , Sudo_Root Team.... & All <3 
