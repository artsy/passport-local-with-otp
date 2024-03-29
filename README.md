# ⚠️ This repo has been deprecated and the code moved directly into [Force](https://github.com/artsy/force/blob/master/src/lib/passport-local-with-otp) 

## passport-local-with-otp

![build status](https://circleci.com/gh/artsy/passport-local-with-otp.svg?style=shield) ![npm version](https://badge.fury.io/js/%40artsy%2Fpassport-local-with-otp.svg)

[Passport] strategy for authenticating with a username,
password and OTP.

This module lets you authenticate using a username, password and time-based
one-time password (OTP) in your Node.js applications.

This is a fork of https://github.com/jaredhanson/passport-local, adapted for the
use case of submitting username, password and otp in unison to an upstream
server for authentication and second-factor verification in a single request.

## Meta

- **State:** production
- **GitHub:** https://github.com/artsy/passport-local-with-otp
- **CI:** [CircleCI]
- **NPM:** New versions of [@artsy/passport-local-with-otp][npm_package] are published on every merge to main via our shared `auto` [configuration][auto_orb]
- **Point People:** [@anandaroop][anandaroop]

## Install

```bash
$ yarn add passport-local-with-otp
```

## Usage

#### Configure Strategy

The local-with-otp authentication strategy authenticates users using a username
password, and otp.  The strategy requires a `verify` callback, which accepts
these credentials and calls `done` providing a user.

```js
passport.use(new LocalWithOtpStrategy(
  function(username, password, otp, done) {
    try {
      const user = authenticateWithUpstreamService(username, password, otp)
      return done(null, user)
    } catch (error) {
      return done(err)
    }
  }
));
```

##### Available Options

This strategy takes an optional options hash before the function, e.g. `new LocalWithOtpStrategy({/* options */, callback})`.

The available options are:

* `usernameField` - Optional, defaults to 'username'
* `passwordField` - Optional, defaults to 'password'
* `otpField` - Optional, defaults to 'otp'

Both fields define the name of the properties in the POST body that are sent to the server.

#### Parameters

By default, `**Local**WithOtpStrategy` expects to find credentials in parameters
named username, password and otp. If your site prefers to name these fields
differently, options are available to change the defaults.

    passport.use(new LocalWithOtpStrategy({
        usernameField: 'email',
        passwordField: 'passwd',
        otpField: 'otp_attempt',
        session: false
      },
      function(username, password, done) {
        // ...
      }
    ));

When session support is not necessary, it can be safely disabled by
setting the `session` option to false.

The verify callback can be supplied with the `request` object by setting
the `passReqToCallback` option to true, and changing callback arguments
accordingly.

    passport.use(new LocalWithOtpStrategy({
        usernameField: 'email',
        passwordField: 'passwd',
        otpField: 'otp_attempt',
        passReqToCallback: true,
        session: false
      },
      function(req, username, password, done) {
        // request object is now first argument
        // ...
      }
    ));

#### Authenticate Requests

Use `passport.authenticate()`, specifying the `'local-with-otp'` strategy, to
authenticate requests.

For example, as route middleware in an [Express](http://expressjs.com/)
application:

```js
app.post('/login',
  passport.authenticate('local', { failureRedirect: '/login' }),
  function(req, res) {
    res.redirect('/');
  });
```

## Examples

Developers using the popular [Express](http://expressjs.com/) web framework can
refer to an [example](https://github.com/passport/express-4.x-local-example)
as a starting point for their own web applications.

## License

[The MIT License](http://opensource.org/licenses/MIT)

Copyright (c) 2011-2015 Jared Hanson <[http://jaredhanson.net/](http://jaredhanson.net/)>

[Passport]: http://passportjs.org/
[CircleCI]: https://app.circleci.com/pipelines/github/artsy/passport-local-with-otp
[anandaroop]: https://github.com/anandaroop
[npm_package]: https://www.npmjs.com/package/@artsy/passport-local-with-otp
[auto_orb]: https://github.com/artsy/orbs/tree/main/src/auto
