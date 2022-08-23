You can add apps to installed_apps this way:

```py
    INSTALLED_APPS=[
        ...
        myApp.apps.MyAppConfigs
        ]
```

### view

```py
def view(request):
    return HttpResponse("ok")
```

### view with template and context

```py
from django.shortcuts import render

from .models import Question


def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    context = {'latest_question_list': latest_question_list}
    return render(request, 'polls/index.html', context)
```

### urlconf

```py
# polls/urls.py

urlpatterns=[
    path('',view,name="poll"), # refer to it using name
]
```

### connect to root URLconf

```py
from django.contrib import admin
from django.urls import include,path
urlpatterns = [
    path('polls/', include('polls.urls')), # connect poll to root
    path('admin/', admin.site.urls), # admin routes used by admin page
]
```

## Create global template to be used by all apps

```py
TEMPLATES = [
    {
        ...
        'DIRS': [os.path.join(BASE_DIR, 'templates')], # new
        ...
    },
]
```
