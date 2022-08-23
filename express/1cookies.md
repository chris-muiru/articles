# Cookies

Simple files stored on users computer.
key value pair
uses:

-  session management
-  personalization
-  user tracking

```
yarn add cookie-parser
```

cookieparser -
middleware which parses cookies attached to the client request object

Cookieparser parses cookie header and populate `req.cookies` with an object keyed by the cookie names

```
# set up new cookie
let cookieparser= require("cookie-parser)
app.use(cookieparser())

app.get('/',(req,res)=>{
    res.cookie('name','express'.send('cookie set'))
})

```

Browser sends back cookies every time it queries the server
View cookie

```
console.log('Cookies: ', req.cookies);
```

### Add cookie with expiration date

```
//Expires after 360000 ms from the time it is set.
res.cookie(name, 'value', {expire: 360000 + Date.now()});
```


Another way to set expiration time is using `maxAge` property. Using this property, we can provide relative time instead of absolute time. Following is an example of this method.
```
//This cookie also expires after 360000 ms from the time it is set.
res.cookie(name, 'value', {maxAge: 360000});
```
### delete a cookie
use `clearCookie` function 
```
res.clearCookie('foo')
```
