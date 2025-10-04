First thing you want to do is always brows the site like a normal user, this means test every function and see what you can do as a normal user, however you need to turn on your intercepter in burp to actually log every request/ respond to e+be able to anaysis it later.

When you got an idea of how the general flow works, look for information.

so i say start with information disclosure:

test /robots.txt

test feroxbuster for hidden files.

look at the url, look for anything you can change(an obvious indicator is equal sign) and observe the behaviour of website.
--
Next we have broken access control:
