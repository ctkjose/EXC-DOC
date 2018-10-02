## Documentation Version 0.1.0 ##
[Documentation Index](./doc_index.md)<br>

## Session Data ##

Session data allows you to store key-value pairs that are preserved across subsequent accesses of your app. The session data is kept as long as the browser is open and survives over page reloads and restores.

Access to session data is dictated by the protocol and domain under which you serve your app.

Setting some session values in PHP:
```PHP
	$client = \exc\client::instance();
	$client->session("fn", "jose");
	$client->session("ln", "Cuevas");
```
In the javascript side we use similar code using app.session():

```JavaScript
	exc.app.session("fn", "Jose");
	exc.app.session("ln", "Cuevas");
```

You can read a session value in javascript using:
```JavaScript
	var name = exc.app.session("fn");
```

In PHP you use:
```PHP
	$name = $client->session("fn");
```

You destroy a session value in JavaScript with:
```JavaScript
	exc.app.sessionRemove("fn");
```
In PHP is similar:
```PHP
	$client->sessionRemove("fn");
```

### Notes ###
Session data is stored using Javascript's [sessionStorage]( https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage).
