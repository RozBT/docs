## Lock: User configurable options
The **Auth0Lock** can be customized through the `options` parameter sent to the `.show()` methods.

```js
var lock = new Auth0Lock('clientID', 'account.auth0.com');

// default signin with signup and reset actions
lock.show(options);

// only signin
lock.showSignin(options);

// only signup
lock.showSignup(options);

// only reset
lock.showReset(options);
```

#### Table of Contents

**For display customization**:
- [connections](#connections-array)
- [dict](#dict-stringobject)
- [container](#container-string)
- [icon](#icon-string)
- [closable](#closable-boolean)
- [socialBigButtons](#socialbigbuttons-boolean)
- [focusInput](#focusinput-boolean)
- [usernameStyle](#usernamestyle-string)
- [gravatar](#gravatar-boolean)
- [disableSignupAction](#disablesignupaction-boolean)
- [signupLink](#signuplink-string)
- [disableResetAction](#disableresetaction-boolean)
- [resetLink](#resetlink-string)
- [popup](#popup-boolean)
- [popupOptions](#popupoptions-object)
- [loginAfterSignup](#loginaftersignup-boolean)
- [rememberLastLogin](#rememberlastlogin-boolean)
- [integratedWindowsLogin](#integratedwindowslogin-boolean)
- [defaultUserPasswordConnection](#defaultuserpasswordconnection-string)
- [defaultADUsernameFromEmailPrefix](#defaultadusernamefromemailprefix-boolean)
- [theme](#theme-string)

**For authentication setup**:
- [callbackURL](#callbackurl-string)
- [responseType](#responsetype-string)
- [forceJSONP](#forcejsonp-boolean)
- [authParams](#authparams-object)
- [sso](#sso-boolean)

### connections {Array}
Array of connections that will be used for the `signin|signup|reset` actions. Defaults to all enabled connections.

```js
// The following will only display
// username and password signin form
lock.show({
  connections: ['Username-Password-Authentication']
});

// ... social connections only
lock.show({
  connections: ['twitter', 'facebook', 'linkedin']
});

// ... enterprise connections only
lock.show({
  connections: ['qraftlabs.com']
});
```

![][connections-image]

### dict {String|Object}

`dict` can be a string matching the language (`'en'`, `'es'`, `'it'`, [etc.][i18n-link]) or an object containing your customized text labels. To learn how to customize error messages, you can [read more here](/libraries/lock/customizing-error-messages).

```js
// select a supported language
lock.show({
  dict: 'es'
});

// or customize the text labels yourself
lock.show({
  dict: {
    signin: {
      title: "Login me in",
      emailPlaceholder: "something@youremail.com"
    }
  }
});
```
![][dict-image]

### container {String}

The `id` of the html element where the widget will be shown. This makes the widget appear inline instead of in a popup.

````html
<div id="hiw-login-container"></div>

<script>
  // initialize
  var lock = new Lock('xxxxxx', '<account>.auth0.com');

  // render in `#hiw-login-container` element
  lock.show({
    container: 'hiw-login-container'
  });
</script>
````

![container][container-image]

### icon {String}

`Url` for an image to load in place of the *Auth0Lock* header badge. Recommended max height of `58px` for a better user experience. Defaults to placeholder image.

```js
// Show default placeholder
lock.show();

// customize with own logo/badge
lock.show({
  icon: 'https://auth0.com/boot/badge.png'
});
```

![][icon-image]

> Note: To disable the header badge at all UI customizations are required. Read the wiki [[article|ui-customization]] on this topic.

### closable {Boolean}

Enable/disable closable feature. Defaults to `true` for modal show and false for embedded.

```js
// closable action enabled by default
lock.show();

// disable the closable action
lock.show({
  closable: false
});
```

![][closable-image]

### socialBigButtons {Boolean}

Force large/small social buttons. Defaults to `true` when no `database` or `ad/ldap` connections enabled and `false` when at least one is configured. Note: setting this property will override previous defaults.

```js
// `false` when at least
// 1 database connection
lock.show();

// `true` when none
lock.show({
  connections: ['facebook', 'linkedin', 'amazon']
});

// force big buttons
lock.show({
  socialBigButtons: true
});

// force small icons
lock.show({
  connections: ['facebook', 'linkedin', 'amazon'],
  socialBigButtons: false
});
```

![][socialbuttons-image]

### focusInput {Boolean}

If true, the focus is set to the email field on the widget. Defaults to `false` when mobile or embedded mode, `true` in other cases.

```js
lock.show({
  focusInput: false
});
```

### usernameStyle {String}

If you don't want to validate that the user enters an email, just set this to `username`. Defaults to `email`

```js
// email as default username style
lock.show();

// force `username` input style
lock.show({
  usernameStyle: 'username'
});
```

![][username-image]

### gravatar {Boolean}

Default: `true`

In `show`, `showSignin` and `showSignup` methods, when user types their email, their associated gravatar picture is displayed in the Lock header.

![][gravatar-image]

### disableSignupAction {Boolean}

Hides the Signup button. Defaults to `true` on `show*()` options and `false` on `.show`.

```js
//
lock.show({
  disableSignupAction: true
});
```

![][disablesignup-image]

### signupLink {String}

Set the URL to be requested when clicking on the Signup button. When set, forces `disableSignupAction` to `false`.

```js
//
lock.show({
  signupLink: 'https://yoursite.com/signup'
});
```

### disableResetAction {Boolean}

Hides the reset password button. Defaults to `true` on `show*()` options and `false` on `.show`.

```js
//
lock.show({
  disableResetAction: true
});
```

![][disablereset-image]

### resetLink {String}

Set the URL to be requested when clicking on the Reset password button. When set, forces `disableSignupAction` to `false`.

```js
//
lock.show({
  resetLink: 'https://yoursite.com/reset-password'
});
```

### popup {Boolean}

If set to true, shows a popup when trying to login with a Social or Enterprise IdP. For more information, [read this](/libraries/lock/authentication-modes#popup-mode). Defaults to `true` when a `callback` is set, otherwise `false`.

```js
lock.show({
  popup: true
});

lock.show({}, function(err, profile) {
  // Popup automatically set to true in this case
});
```

![][popup-image]

### popupOptions {Object}

Options for the `window.open` [position and size][windowopen-link] features. This only applies if `popup` is set to true.

```js
lock.show({
  popup: true,
  popupOptions: { width: 300, height: 400, left: 200, top: 300 }
});
```

![][popupoptions-image]

### loginAfterSignup {Boolean}

Triggers a sign in call after sign up. Defaults to `true`.

```
// will sign in user after sign up
lock.show();

// won't sign in user after sign up
lock.show({
  loginAfterSignup: false
});
```

### rememberLastLogin {Boolean}

Request for SSO data and enable **Last time you signed in with[...]** message. Defaults to `true`.

```js
// rememberLastLogin is enabled by default
lock.show();

// and this way you can disable it
// to force for input credentials
lock.show({
  rememberLastLogin: false
});
```

![][remember-image]

### integratedWindowsLogin {Boolean}

Allows for Realm discovery by `AD`, `LDAP` connections. Defaults to `true`.

```js
// AD|LDAP Realm discovery enabled by default
lock.show();

// disable Realm discovery to force
// input of credentials
lock.show({
  integratedWindowsLogin: false
});
```

### defaultUserPasswordConnection {String}

When multiple Database/AD-LDAP connections, specify which one should be used with the Email/Password fields. Defaults to the first Database connection found (if exists) or the first AD-LDAP connection found.
> Shall be renamed to just `forceDatabase`.

```js
// defaults to the first configured
// database connection in Auth0's dashboard
// configured are `production-database`,
// `staging-database` and `test-database`
lock.show();

// defaults to the first on the list
// of provided database connections
lock.show({
  // `production-database` not listed
  connections: ['staging-database', 'test-database']
});

// force `test-database`
lock.show({
  defaultUserPasswordConnection: 'test-database'
});
```

### defaultADUsernameFromEmailPrefix {Boolean}

Resolve the AD placeholder username from the email's prefix. Defaults to `true`.

```js
// default username from email prefix
lock.show();

// does not fill username input
lock.show({
  defaultADUsernameFromEmailPrefix: false
});
```

![][defaultadusernamefromemailprefix-image]

### theme {String}

This property allows to change the default `a0-theme-default` class to whatever delivered by this option. The result will be a containing CSS class like `a0-theme-<theme-value>`. The result of this will be a complete style reset of the Lock allowing to set your own theme/styles. Defaults to `default`.

```js
// Default theme out of the box
lock.show();

// Theme reset
lock.show({ theme: false });

// Theme rename to `a0-theme-mycroft`
lock.show({ theme: 'mycroft' });
```

### callbackURL {String}

The url auth0 will redirect back after authentication. If not set, it defaults to `location.href`. If you don't set `callbackURL`, [responseType](#responsetype-string) will default to `token`.

```js
lock.show({
  callbackURL: 'http://mydomain.com/callback'
});
```

### responseType {String}

Should be set to `token` for *Single Page Applications*, otherwise `code`. Defaults to `token` if `callbackURL` is __not__ set, `popup` mode is set to true or if a `callback` is supplied. Otherwise, it'll be `code`.

```js
lock.show({
  responseType: 'token'
});
```

### forceJSONP {Boolean}

Force JSONP requests for all `auth0-js` instance requests. This setup is useful when no CORS allowed. Defaults to `false`.

```js
lock.show({
  forceJSONP: true
});
```

### authParams {Object}

You can send parameters when starting a login by adding them to the options object. The example below adds a `state` parameter with a value equal to `foo`. [[Read here|Sending-authentication-parameters]] to learn more about what `authParams` can be set.

```js
lock.show({
  // ... other options ...
  authParams: {
    state: 'foo'
  }
});
```

> Note: For a full spec on every supported parameter check the wiki [article][authparams-link] on this topic.

### sso {Boolean}

Sets a cookie used for single sign on. The cookie will be used later to allow `rememberLastLogin` display the **Last time you signed in with ...** message. This only applies to Database Connections when using `popup: true` and fires a popup where authentication takes place. Last but not least, it prompts for a multifactor authentication code, if enabled.

> Warning: Failing to set this to true will result in multifactor authentication not working correctly.

```js
lock.show({
  sso: true
});
```

***

## Internally resolved

The following are all internal options. The only reason they are listed here is to have a full documentation for the options object.

> Note: passing any of the following to the `options` object will be overridden by `Auth0Lock`s options manager. Do not attempt to modify these... it won't happen.

### mode {String}

Set the `show` mode for the display. Allowed values are `signin`, `signup` and `reset`. Defaults to `signin`.

### popupCallback {Function}

Internally set from `callback` parameter



[connections-image]: https://cloudup.com/c5lcry6Cj9C+
[container-image]: https://cloudup.com/cs2FwrQg-Fh+
[icon-image]: https://cloudup.com/ca-expy_GvW+
[dict-image]: https://cloudup.com/cfQWrtVACO3+
[remember-image]: https://cloudup.com/cUqUZgG_apa+
[disablesignup-image]: https://cloudup.com/cdHWtzNrCAi+
[disablereset-image]: https://cloudup.com/cnolm6HqGMq+
[username-image]: https://cloudup.com/cYAiKnsV_EQ+
[authparams-link]: /libraries/lock/sending-authentication-parameters
[closable-image]: https://cloudup.com/cySs9OP6yUc+
[socialbuttons-image]: https://cloudup.com/cMsyrifz46s+
[popup-image]: https://cloudup.com/cY08amfJ1yh+
[windowopen-link]: https://developer.mozilla.org/en-US/docs/Web/API/Window.open#Position_and_size_features
[popupoptions-image]: https://cloudup.com/cqvq_Gz5VUZ+
[i18n-link]: https://github.com/auth0/lock/tree/master/i18n
[gravatar-image]: https://cldup.com/yt3vJ2UQzq.png
[defaultadusernamefromemailprefix-image]: https://cldup.com/IWdks93Oh5.png