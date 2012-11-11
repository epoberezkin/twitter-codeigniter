twitter-codeigniter
===================

CodeIgniter Twitter OAuth library

This library encapsulates TwitterOAuth library by Abraham Williams, which is probably the best and easiest to use Twitter OAuth library
It is available here: https://github.com/abraham/twitteroauth

I could not find a readily available library for CodeIgniter so I hed to do it...

It is for lazy people like me for whom examples provided by Abraham Williams seem to complicated

TwitterOAuth library (only twitteroauth folder from github) should be located in /application/libraries/twitteroauth/ folder

It is included in this repository, I hope Mr Abrahams doesn't mind. But please make sure you download the latest version.


This library simplifies Twitter login and access by wrapping TwitterOAuth methods in its own class.

This library is a bit of a hack - it stores data in the session and you have to load it three times to pass through authentication process.

You will see in example how it works - for me it seems much easier to use than original TwitterOAuth library.


The directory structure is the same as in CodeIgniter framework.

application/conrollers/twtest.php - the controller of the test application. It does not use any views.
application/config/twitter.php - Twitter app config.
application/libraries/twconnect.php - CodeIgniter Twitter OAuth library.