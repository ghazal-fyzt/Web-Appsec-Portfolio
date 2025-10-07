First thing you want to do is always brows the site like a normal user, this means test every function and see what you can do as a normal user, however you need to turn on your intercepter in burp to actually log every request/ respond to e+be able to anaysis it later.

When you got an idea of how the general flow works, look for information.

so i say start with information disclosure:

test /robots.txt

test feroxbuster for hidden files. 

feroxbuster -u https://TARGET -w [wordlist path] -t 50 --insecure -s 200,301,302,401,403 -f --redirects --proxy http://127.0.0.1:8080

Recommended Wordlist: GitHub-Link


look at the url, look for anything you can change(an obvious indicator is equal sign) and observe the behaviour of website.

broken access:


at the end of the day no matter the backend language everything will be renderd in html so the site can understand it therefore an important part is to always checkthe html code.

X-Original-URL: if the app trusts this header from the client, an attacker can influence routing, redirects, caching, or access checks — leading to open redirects, cache poisoning, or incorrect access decisions.
ex: X-Original-URL: /admin


