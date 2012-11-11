twitter-codeigniter
===================

##CodeIgniter Twitter OAuth library##

This library encapsulates [__TwitterOAuth__ library](https://github.com/abraham/twitteroauth) by _Abraham Williams_,
which is probably the best and the easiest to use Twitter OAuth library
(at least it is the only one I could understand and I tried two other libraries before, which seemed simple enough).
I could not find a readily available twitter library for [CodeIgniter](http://codeigniter.com) so I did __Twconnect__...
It is for lazy people like me for whom examples provided by Abraham Williams seem too complicated.

This library simplifies Twitter login and access by wrapping TwitterOAuth methods in its own class.

It is a bit of a hack - it stores data in the CodeIgniter session and
you have to load it three times to get through authentication process.
But it does the work and for me it seems much easier to use than original TwitterOAuth library.

If you want to see how test application using __Twconnect__ library works please feel free to try it here:
http://beta.whoyoumeet.com/twtest.
When you click _Connect to Twitter_ it will try to authenticate my [__Who You Meet__](http://whoyoumeet.com)
application for read-only access to your Twitter account.
You can consider it a shameless promotion of the idea I am working on.


##The directory structure##

It is the same as in CodeIgniter framework - all files should be in the same directories of the framework.

```/application/conrollers/twtest.php``` - the controller of the test application.
It does not use any views and it is really simple. It shows all __3 stages__ of the authentication flow.

```/application/config/twitter.php``` - your Twitter app configuration file. Your app token/secret should be here.

```/application/libraries/twconnect.php``` - CodeIgniter Twitter OAuth library.

```/application/libraries/twitteroauth/``` - TwitterOAuth library (only twitteroauth folder from github) should be located here.
It is included in this repository, I hope Mr Abrahams doesn't mind. But please make sure you download the latest version from the link above.


##CodeIgniter files##
This test application and the library assume that you have ```session``` library and ```url``` helper autoloaded -
you can do it in ```/application/config/autoload.php``` file in CodeIgniter framework.
To make sessions work you need to set encryption key in ```/application/config/config.php``` - I keep forgetting about it.



##The library class functions##

You call all functions after you load the library this way: ```$this->twconnect->function_name()```

1. ```Twconnect()``` - Twconnect class constructor.
It handles creation of the TwitterOAuth object for all 3 stages of authentication process.
You call it by loading the library ```$this->load->library('twconnect')``` and it takes access/request token data from CodeIgniter session.

2. ```twredirect($callback_path)``` - redirects to Twitter for authetication.
Callback path is the path segment containing controller and function of your application
that Twitter should redirect to after authentication.

3. ```twprocess_callback()``` - requests permanent access token after callback.
The way this library is designed, you MUST call this method after Twitter redirects back.
It returns true if authentication was successful and false if it was not.

4. ```tw_get($url, $parameters)``` - GET request to Twitter API.
Checks if authenticated, otherwise returns false.

5. ```tw_post($url, $parameters)``` - POST request to Twitter API.
Checks if authenticated, otherwise returns false.
 
6. ```twaccount_verify_credentials()``` - gets all Twitter user info and saves it to $this->tw_user_info for future use.

All GET and POST requests are here https://dev.twitter.com/docs/api/1.1
(everybody knows it but I keep forgetting where it is, so I put it here).

In addition to these functions you can access all functions from __TwitterOAuth__ library, __Twconnect__ extends this class.



##The library class variables##

You access all variables after you load the library this way: ```$this->twconnect->variable_name```

1. ```tw_user_id``` - Twitter user ID ( *null*  if not authenticated ).

2. ```tw_user_name``` - Twitter user screen name ( _null_  if not authenticated ).

3. ```tw_user_info``` - the whole bunch of info about user.
```twaccount_verify_credentials()``` should be called to set it.
You will see what info is there when you try _twtest_ application.

4. ```tw_config``` - copy of the configuration file ```/application/config/twitter.php```

5. ```tw_request_token``` - temporary request token, array.

6. ```tw_access_token``` - permanent access token for the currently logged in user.
It should be stored in/retrieved from your users table in the database.
If the current user has been previously authenticated by Twitter and you have previously saved access token
you should set ```tw_access_token``` in CodeIgniter Session before loading __Twconnect__ library.
You can see what data is stored in session after authentication by going to /twtest - it shows session data.
At least one of  two variables (```tw_request_token``` and ```tw_access_token```) will be null,
depending on where you are in authentication process.

7. ```tw_status``` - Twitter user status:
  * ```'old_token'``` - request token expired, should be redirected to Twitter again.
with ```twredirect($callback_path)``` function.
  * ```'access_error'``` - could not connect to Twitter to get access token. Can be just Internet connection problem.
  * ```'verified'``` - user successfully verified by Twitter.