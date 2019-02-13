
 - http://37.187.186.219:5555 -- Our TARGET !
* inspecting the source... nothing's interesting there 

* So i saved the picture (whiterabbit.jpg) and i started analysing it , i found this area "morpheus.matrix:5555"
 ![alt text](https://i.imgur.com/EjZ3azD.png "huh")
 
> Maybe something behind this adresse, so i made a little python script to communicate with our target.


>import requests
>url = "http://37.187.186.219:5555"
>head1 = {
>    'host': "morpheus.matrix:5555",  ##  The host Header tells our target  which virtual host to use.
>    'access-control-request-headers': "application/x-www-form-urlencoded",
>    'x-requested-with': "application/json",
>    'cache-control': "no-cache",
>    }	
>r = requests.request("GET", url, headers=head1)
>print(r.text)

> C:\>Python Matrix.py
> ================================= Result ====================================
> <body bgcolor=#001B33><center><pre><font size=4 color=#2CAC0C face=monospace>
> Welcome my name is morpheus 
> Here is the blue pill : MCTF3{WeARe4LL_our_Own_Perception}
> you can take this pill and stop here or you can take the red one to see the truth
> this one is hidden, ask the program for it
> pill color:  369296fbbb4187530420a169398fc793<!--
> ./matrix.txt
> !-->
> <img src="./pill.jpeg" height=250>
> </center></pre>

> - Cool , we got our first Flag ! MCTF3{WeARe4LL_our_Own_Perception} :D

