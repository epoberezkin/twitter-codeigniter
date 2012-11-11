twitter-codeigniter
===================

**CodeIgniter Twitter OAuth library**

This library encapsulates TwitterOAuth library by Abraham Williams, which is probably the best and easiest to use Twitter OAuth library
It is available here: https://github.com/abraham/twitteroauth

I could not find a readily available twitter library for CodeIgniter so I did it...

It is for lazy people like me for whom examples provided by Abraham Williams seem too complicated

------------------

This library simplifies Twitter login and access by wrapping TwitterOAuth methods in its own class.

This library is a bit of a hack - it stores data in the session and you have to load it three times to pass through authentication process.

But it does the work and for me it seems much easier to use than original TwitterOAuth library.

-----------------

**The directory structure is the same as in CodeIgniter framework**

_/application/conrollers/twtest.php_ - the controller of the test application. It does not use any views and it is really simple. It shows all the authentication flow

_/application/config/twitter.php_ - your Twitter app config.

_/application/libraries/twconnect.php_ - CodeIgniter Twitter OAuth library.

_/application/libraries/twitteroauth/_ - TwitterOAuth library (only twitteroauth folder from github) should be located here

It is included in this repository, I hope Mr Abrahams doesn't mind. But please make sure you download the latest version from the link above.
