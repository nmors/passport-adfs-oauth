# Passport-Adfs-OAuth

[Passport](http://passportjs.org/) strategy for authenticating with the ADFS 3.0 OAuth 2.0 API.

This module lets you authenticate using ADFS 3.0 in your Node.js applications.
By plugging into Passport, ADFS 3.0 authentication can be easily and unobtrusively integrated into any application or framework that supports [Connect](http://www.senchalabs.org/connect/)-style middleware, including [Express](http://expressjs.com/).

## Installation

    $ npm install passport-adfs-oauth

## Usage

#### Configure Strategy

The ADFS OAuth authentication strategy authenticates users using a Microsoft ADFS 3.0
account using OAuth 2.0.  The strategy requires a `verify` callback, which
accepts these credentials and calls `done` providing a user, as well as
`options` specifying a client ID, client secret, tenant id, resource and redirect URL.

    passport.use(new ADFSOAuthStrategy({
        loginUrl: EXAMPLE_URL,
        clientId	: EXAMPLE_CLIENT_ID,
    	clientSecret: EXAMPLE_CLIENT_SECRET,
		tenantId 	: '',
		resource 	: EXAMPLE_CLIENT_RESOURCE,
		redirectURL : EXAMPLE_CALLBACK_RedirectURL
      },
      function(accessToken, refreshToken, profile, done) {
      	return done(err, user);
      }
    ));

* clientId : Id of the registered azure online application.
* clientSecret : Password of the registered azure online application.
* tenantId : Not needed for ADFS
* resource : The resource server that the Client wants an access token to, as registered in the Identifier parameter of the Relying Party trust.
* redirectURL : The redirect uri that is associated with the Client. Must match the RedirectUri value associated with the Client in ADFS.
E.g
	```javascript  

        clientId	: ADFSOAuth_ClientId,
    	clientSecret: ADFSOAuth_ClientSecret,
		tenantId 	: ADFSOAuth_AppTenantId,
		resource 	: ADFSOAuth_AuthResource,
		redirectURL : ADFSOAuth_RedirectURL,
		proxy : {
			host : 'myProxyHost',
			port : 'myProxyPort',
			protocol : 'https' // http / https
		},
		myParameter : 'Im a parameter'

    ));
	```  

The callback url looks like <br>

	"redirectURL + '?redirectUrl=' + redirectUrl + "&" + myParameter="Im a parameter"

* The proxy settings passed through the oauth2 module, wich handles the authorization requests.

#### Authenticate Requests

Use `passport.authenticate()`, specifying the `'adfsoauth'` strategy, to
authenticate requests.

For example, as route middleware in an [Express](http://expressjs.com/)
application:

    app.get('/auth/adfsoauth',
      passport.authenticate('adfsoauth'),
      function(req, res){
        // The request will be redirected to SharePoint for authentication, so
        // this function will not be called.
      });

    app.get('/auth/adfsauth/callback',
      passport.authenticate('adfsoauth', {
		failureRedirect: '/login',
		refreshToken: ADFSOAuth_RefreshToken
	  }),
      function(req, res) {
        // Successful authentication, redirect home.
        res.redirect('/');
      });

* refreshToken : After one initial authentication, you get a refresh token. You can pass it to the authenticate method to simply renew your access token without a new callback.
## Credits

  - [QuePort](https://github.com/QuePort)
  - [Thomas Herbst](https://github.com/macrauder)
  - [Tobias Winkler](https://github.com/Tschuck)

## License

(The MIT License)

Copyright (c) 2013

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
