### Sessions

http is stateless. to associate a request to any other request,you need a way to store user data btn http requests. using session,you assign a client an id and it makes all further requests using that id.
we will need the express-session middleware and cookie parser

```bash
yarn add express-session
```

The session middleware handles all things for us, i.e., creating the session, setting the session cookie and creating the session object in `req` object.

```js
var express = require("express")
var cookieParser = require("cookie-parser")
var session = require("express-session")

var app = express()

app.use(cookieParser())
app.use(session({ secret: "Shh, its a secret!" }))

app.get("/", function (req, res) {
	if (req.session.page_views) {
		req.session.page_views++
		res.send("You visited this page " + req.session.page_views + " times")
	} else {
		req.session.page_views = 1
		res.send("Welcome to this page for the first time!")
	}
})
app.listen(3000)
```

Whenever we make a request from the same client again, we will have their session information stored with us (given that the server was not restarted). We can add more properties to the session object. In the following example, we will create a view counter for a client.

`Note`: in resent versions,you dont need to call cookieparser() middleware. Session middleware now reads and writes cookies directly on `req/res`.

`memoryStore` is purposely not designed for production env. we will check out session stores later

## useful session options
- **name** - name of session id cookie to set in the response(and read from request).
- **resave** - force session to be saved back to the session store,even if the session was never modified during the request.
- **secret**  - secret used to sign the session id cookie.
- **cookie** - settings object for session ID cookie.
   > default - { path: '/', httpOnly: true, secure: false, maxAge: null }.
   
   options to set this option.
   - **cookie.expires**  - specifies the `date` object to be value of `expires set-cookie`
   - **cookie.maxAge** - number of milliseconds it will take for cookie to expire
   - **cookie.path** - value for the Path `set-cookie`. by default it is `\`