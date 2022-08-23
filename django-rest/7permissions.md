- `permissions` - determines whether a request should be granted or denied access. 
- typically uses info from `request.user` and `request.auth`   properties 

### how permissions are determined
- permissions in rest framework are always defined as a list of permission classes.
before running main body of view,each permission in the list is checked. if any permission check fails,an `exceptions.PermissionDenied` or `exceptions.NotAuthenticated` exception will be raised, and the main body of the view will not run.
When the permission checks fail, either a "403 Forbidden" or a "401 Unauthorized" response will be returned, according to the following rules:

-    The request was successfully authenticated, but permission was denied. — An HTTP 403 Forbidden response will be returned.
-   The request was not successfully authenticated, and the highest priority authentication class does not use WWW-Authenticate headers. — An HTTP 403 Forbidden response will be returned.
-    The request was not successfully authenticated, and the highest priority authentication class does use WWW-Authenticate headers. — An HTTP 401 Unauthorized response, with an appropriate WWW-Authenticate header will be returned.

## setting permission policy
### set default policy globally
```py
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
    ]
}
```
default: `AllowAny`

### set per view
```py
from rest_framework.decorators import api_view, permission_classes
from rest_framework.permissions import IsAuthenticated
from rest_framework.response import Response
@api_view['GET]
@permission_classes([IsAuthenticated])
def view(request,format=None):
    content={'status':'request was permitted'}
    return Response(content)
```
**Note**: when using thing,django ignores the default global

## common permissions
- AllowAny
- IsAuthenticated
- IsAuthenticatedOrReadOnly 
- IsAdminUser

## Custom permissions
- to implement custom permission override `BasePermission` and implement either or both of following:
```py
.has_permission(self,request,view)
.has_object_permission(self,request,view,obj)
```
method should return `True` if request should be granted access and `False` otherwise

If you need to test if a request is a read operation or a write operation, you should check the request method against the constant `SAFE_METHODS`, which is a tuple containing 'GET', 'OPTIONS' and 'HEAD'. For example:

```py
if request.method in permissions.SAFE_METHODS:
    # Check permissions for read-only request
else:
    # Check permissions for write request
```
customPermissions will raise a `PermissionDenied` exception if test fails
To change the error message  with exception,implement `message` attribute directly on your custom permission.
otherwise `default_detail` attribute from `PermissionDenied` will be used.To change code identifier associated with the exception,implement a code attribute directly on your custom permission,otherwise `default_code` will be used
```py
from rest_framework import permissions

class CustomerAccessPermission(permissions.BasePermission):
    message = 'Adding customers not allowed.'
    code=...

    def has_permission(self, request, view):
         ...

```
### example
The following is an example of a permission class that checks the incoming request's IP address against a blocklist, and denies the request if the IP has been blocked.
```py
from rest_framework import permissions

class BlocklistPermission(permissions.BasePermission):
    """
    Global permission check for blocked IPs.
    """

    def has_permission(self, request, view):
        ip_addr = request.META['REMOTE_ADDR']
        blocked = Blocklist.objects.filter(ip_addr=ip_addr).exists()
        return not blocked

```
As well as global permissions, that are run against all incoming requests, you can also create object-level permissions, that are only run against operations that affect a particular object instance. For example:
```py
class IsOwnerOrReadOnly(permissions.BasePermission):
    """
    Object-level permission to only allow owners of an object to edit it.
    Assumes the model instance has an `owner` attribute.
    """

    def has_object_permission(self, request, view, obj):
        # Read permissions are allowed to any request,
        # so we'll always allow GET, HEAD or OPTIONS requests.
        if request.method in permissions.SAFE_METHODS:
            return True

        # Instance must have an attribute named `owner`.
        return obj.owner == request.user
```