# A3M  
### 

A3M (Account Authentication & Authorization) is a CodeIgniter 3.x package that leverages bleeding edge web technologies 
like OpenID and OAuth to create a user-friendly user experience. It gives you the CRUD to get working right away 
without too much fuss and tinkering! Designed for building webapps from scratch without all that tiresome 
login / logout / admin stuff thats always required.

## Original Authors

**Jakub** [@kubanishku](https://twitter.com/kubanishku/)  
**PengKong** [@pengkong](https://github.com/pengkong)
		
## Key Features & Design Goals

* Native Sign Up, Sign In with 'Remember me' and Sign Out  
* Native account Forgot Password and Reset Password  
* Facebook/Twitter/Google/Yahoo/OpenID Sign Up, Sign In and Sign Out  
* Manage Account Details, Profile Details and Linked Accounts  
* reCAPTCHA Support, SSL Support, Language Files Support  
* Gravatar support for picture selection (via account profile) **(NEW)**
* Create a painless user experience for sign up and sign in  
* Create code that is easily understood and re-purposed  
* Utilize Twitter Bootstrap (a fantastic CSS / JS library)  
* Graceful degradation of JavaScript and CSS  
* Proper usage of CodeIgniter's libraries, helpers and plugins  
* Easily Configurable via config file  

## Folder structure  

* `/application/` - what you should be editing / creating in    
* `/system/` - default CodeIgniter system folder (don't touch!)   
* `/resource/` - css / images / javascript (folder configurable via `constants.php`)   
* `/user_guide/` - latest guide for CI (can be deleted, just for CI reference)

## 3rd Party Libraries & Plugins

* [recaptcha_pi.php](http://code.google.com/p/recaptcha/) - recaptcha-php-1.11
* [facebook_pi.php](https://github.com/facebook/facebook-php-sdk/) - v.3.2.2 
* [twitter_pi.php](https://github.com/jmathai/twitter-async) - Updated to latest release - [Jun 21, 2013](https://github.com/jmathai/twitter-async/commits/master)  
* [phpass_pi.php](http://www.openwall.com/phpass/) - Version 0.3 / genuine _(latest)_ 
* [openid_pi.php](http://sourcecookbook.com/en/recipes/60/janrain-s-php-openid-library-fixed-for-php-5-3-and-how-i-did-it) - php-openid-php5.3  
* [gravatar.php](https://github.com/rsmarshall/Codeigniter-Gravatar) - codeigniter (6/25/2012) rls

## Dependencies

* CURL
* DOM or domxml 
* GMP or Bcmatch

## Installation Instructions
Check out our wiki: https://github.com/donjakobo/A3M/wiki/Installation-Instructions
for help on getting started.

<<<<<<< HEAD
+ Download the latest version of [A3M](https://github.com/donjakobo/A3M/)
+ Extract to a folder accessible on your webserver (`/` or something like `/a3m/` )  
+ Create a database by importing `a3m_database.sql` script found it root folder of package  
+ Configure `/application/config/config.php` & `database.php` to match your CI setup (domain + database credentials)  
+ Modify `.htaccess` file if your app location is different than `/` (example: `domain.com/a3m/`)  
+ Configure `/application/config/account/*` files to reflect your setup (reCAPTCHA, twitter, facebook, openid providers, etc;)

### Twitter configuration:
##### Twitter site (`https://dev.twitter.com/apps`)
+ Create an App and note down the "Consumer key" and "Consumer secret" values
+ Callback URL: `https://www.yoursite.com/account/connect_twitter/`
+ Allow this application to be used to Sign in with Twitter [X]

##### A3M
+ Edit `application/config/account/twitter.php` and insert your consumer key and consumer secret.

##### Testing on localhost
+ localhost and 127.0.0.1 will not work. Use your internal IP (eg. 192.168.1.10)

### Facebook configuration:
##### Facebook Developers site (`https://developers.facebook.com/apps`)
+ Create new App
+ Note down "App ID" and "App Secret" values
+ Tick "Website with Facebook Login" URL: `http://www.yoursite.com`

##### A3M
+ Edit `application/config/account/twitter.php` and insert your consumer key and consumer secret.

##### Testing on localhost
+ Facebook login seems to only work on a live environment (see https://github.com/donjakobo/A3M/issues/3)

### Google / OpenID configuration:
+ Those should work out of the box. No further configuration needed.

##### Testing on localhost
+ Some webservers (XAMMP) have outdated certificates. If you get a `Fatal error: Call to a member function addExtension() on a non-object in` error you must do the following:
	
	edit 
	`application/helpers/account/Auth/Yadis/ParanoidHTTPFetcher.php` and add
	`curl_setopt($c, CURLOPT_SSL_VERIFYPEER, false);` after line 140 (before `curl_exec($c);`)

	**WARNING: DO NOT DO THIS ON YOUR PRODUCTION/LIVE WEB SERVER AS THIS LEAVES YOUR SERVER VURNERABLE TO MITM ATACKS**

### Yahoo! configuration:
+ Those should work out of the box. No further configuration needed.

##### Testing on localhost
+ Testing on localhost works without any changes.

## Authorization, Roles, and Permissions:

+ Connect to your database and insert a new row into the "a3m_rel_account_role" with the Role ID for Admin (by default this is "1") and the Account ID you want to give Admin Rights to.
+ After you login to the website you should see a few new options under your account for Manage Users, Manage Roles, and Manage Permissions.

### Example: Create an Authors Role with permissions to "Post New Articles".

+ Go to "Manage Roles" and create the new "Authors" role. 
  + Name: Authors
  + Description: Website Authors that are allowed to post new articles.
  + Permissions: None
+ Jump to "Manage Permissions" and create the "Post New Articles" permission: 
  + Key: post_articles
  + Description: Post New Articles
  + Roles: Check the "Authors" Role
+ Now you can check if the currently logged in user has access to certain features in your Controllers. You simply pass in the "Key" of the permission you created, in this case that is "post_articles".
    
  `$this->authorization->is_permitted('post_articles'); //returns boolean value`


## Note
<<<<<<< HEAD
+ Please fork and help out! Only with your help will this keep growing and getting better.
=======
+ The current codebase is _semi-stable_ due to a large re-write effort of the original application and this branch attempt to bring it to CodeIgniter 3. Please fork and help out!
>>>>>>> pr/60
+ Note that twitter doesn't work if your base url is `localhost` and facebook won't work if your base url is `127.0.0.1`. Therefore ensure that your base url is something like `yoursite.com`. One way to do that is to simply [map the hostname](http://en.wikipedia.org/wiki/Hosts_%28file%29) your want to `127.0.0.1` on your development machine.
Your twitter callback URL should take into account whether or not you have enabled SSL in your a3m config   
 + `https://domain.com/account/connect_twitter` (SSL **Enabled**) 
 + `http://domain.com/account/connect_twitter` (SSL Disabled) 

Configuring this wrongly will result in an `EpiOAuthUnauthorizedException` exception being thrown.

## Guide

Bellow you'll find guide to the different A3M libraries. This guide assumes, that you have all the corresponding models and helpers as well.
+ This guide was created by [@AdwinTrave](https://github.com/AdwinTrave) on GitHub.

For starters you should always include `maintain_ssl();` on your pages. In order to maintain your ssl if you have it enabled.

### Authentication

This library makes all the user authentications.

#### is_signed_in()

Returns a boolean value after it checks the session data, that the user is signed in.

#### sign_in()

Signes in user and redirects to given page, either via session data or GET.

Three variables are needed to be passed in:
+ Username/email
+ Password
+ Remember me?

So the code to call to this method will look something like this:

```php
$this->authentication->sign_in($this->input->post('sign_in_username_email', TRUE), $this->input->post('sign_in_password', TRUE), $this->input->post('sign_in_remember', TRUE))
```

If the password and username are correct it will login the user and will redirect to the home page, or it will redirect the user to the page that has been passed via `GET` `continue` or via session session `sign_in_redirect`.

If the login attempt fails for any reason, it will return boolean value of FALSE and increase the session counter of failed attempts, which you can access under `sign_in_failed_attempts`. To make a check that the user didn't pass over the limit you can call this in an if statement:
```php
$this->session->userdata('sign_in_failed_attempts') < $this->config->item('sign_in_recaptcha_offset')
```

Lastly "Remember me?" is a booblean variable which will keep the user signed in for a longer period of time.

#### sign_out()

As name suggests this method signs out the user and destroyes any session data related to that user and redirects to the homepage.

### Authorization

#### is_permitted()

This method has two input variables:

+ Permission key
+ Require all

Permission key can be either one permission value or array of values. If you use an array of values then use the second boolean variable to determine if the user needs to have permission to use all of those keys in order to get access.

Will return boolean value based on if the user has permission for the given key.

#### is_admin()

This method will check if the user is admin.

#### is_role()

You pass in the name of the role and the function will determine if the user has that role.
=======
## Help and Support  
* Found a bug? Try forking and fixing it. 
* Open an issue if you want to discuss/highlight it
* Go to StackOverflow under the tag `codeigniter-a3m` http://stackoverflow.com/questions/tagged/codeigniter-a3m if you have implementation issues (installation problems, etc;)

>>>>>>> a71ba9827ab46993ab89fdd65aac30c359922290
