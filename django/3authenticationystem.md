## User objects
core of authentication system. rep those interacting with site

### Attributes of User objects
- username
- password
- email 
- first_name
- last_name

### creating users
```py
from django.contrib.auth.models import User
user=User.objects.create_user('john','lonnon@gmail.com','password')
# at this point,user is a User objects saved to db
# you can change its attributes if you want to

user.last_name='dvinsi'
user.save()
```
### creating superusers
```bash
python manage.py createsuperuser
```

### changing password
- django stores the hashed password,hence dont attempt to manipulate the password attribute of user directly.
We have several ways of changing passwords

1. ```bash manage.py changepassword *username* ```
2. 
```py
from django.contrib.auth.models import User
u=User.objects.get(username='john')
u.set_password('new password')
u.save()
```

## Authenticating users
`authenticate(request=none,***credentials)`
checks the credentials against each authenticated backend and returns a `User` object if the credentials are valid for a backend. If credentials are not valid for any backend or if backend raises a `PermisionDenied`,it returns `none`
```py
from django.contrib.auth import authenticate
user = authenticate(username='john', password='secret')
if user is not None:
    # A backend authenticated the credentials
else:
    # No backend authenticated the credentials
```
- `request` is an optional HttpRequest which is passed on the 

- `authenticate()` method of the authentication backends.



## check if user is authenticated
- `request.user.is_authenticated` - boolean(`True or False`)

## login user

`login()` attach authenticated user to current session
To log a user in, from a view, use `login()`. It takes an `HttpRequest` object and a `User` object. `login()` saves the user’s ID in the session, using Django’s session framework.
```py
from django.contrib.auth import authenticate, login

def my_view(request):
    username = request.POST['username']
    password = request.POST['password']
    user = authenticate(request, username=username, password=password)
    if user is not None:
        login(request, user)
        # Redirect to a success page.
        ...
    else:
        # Return an 'invalid login' error message.
        ...

```
## logout user
`logout(request)`
To log out a user who has been logged in via `django.contrib.auth.login()`, use `django.contrib.auth.logout()` within your view. It takes an `HttpRequest` object and has no return value. Example:
```py

from django.contrib.auth import logout

def logout_view(request):
    logout(request)
    # Redirect to a success page.

```
Note that `logout()` doesn’t throw any errors if the user wasn’t logged in.