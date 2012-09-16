That object is made for sending clientside errors or messages to some server(s).

Implemented two ways of logging an error or message:
* to google analytics account and
* to some server via generic image.
That can happen simultaneously.

This javascript code is meant to be included somewhere in the head of the document before any other javascript is loaded and run to properly log errors while loading other scripts.
It does not have to be saved to some *.js-file because an error can occur while loading that file.
This peace of code has to be inserted by server side on every page where logging is needed.

## Initialization
This **object must be initialized** and you have to provide initialization object to init method like that:
```javascript
	(function()
	{
		Logger.init({
			sendErrorsToServer: '/index.html?action=log_client_error',
			sendErrorsToGA: true,
			sendMessagesToServer: '/index.html?action=log_client_message',
			sendMessagesToGA: true
		});
	}());
```
You can provide one to four parameters:
- to send errors and messages to server - **sendErrorsToServer** and **sendMessagesToServer** respectively (a string - url for sending logs)
- to send errors and messages to google analytics account - **sendErrorsToGA** and **sendMessagesToGA** respectively (boolean).
Any parameter can be omitted.

## Usage examples

### Logging a message
```javascript
Logger.sendMessage('just message to log');
```

### Error logging

* in catch statement
	```javascript
	try {
		throw new Error('throwing an error with message');
	}
	catch (e) {
		Logger.sendError(e);
	}
	```

* by hand, when we just think it's an error for any reason
	```javascript
	Logger.sendError('error message by hand');
	```

* uncaught errors are sent to server too (trythis code to get undefined property)
	```javascript
	window.str.t;
	```


## What needs to be done on the other side
### Receiving errors on server side
needs some serverside language (Node, Python, PHP, Ruby or something else) to log it into some database or to a file.


### For viewing errors and messages in your google analytics account
you need to proceed to **Standard Reporting** &rarr; **Content** &rarr; **Events** &rarr; **Overview** &rarr; **Custom Javascript Message**(**Custom Javascript Error**).

And of course google analytics must be enabled with a standard code like that:
```javascript
var _gaq = _gaq || [];
_gaq.push(['_setAccount', 'UA-XXX']);
_gaq.push(['_trackPageview']);

(function() {
    var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
    ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
    var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
})();
```
