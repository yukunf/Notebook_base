# Django Tutorial 3 - Implement Pages

## 2.1 User Registration App

Remember to add configure to the whole app list in `settings.py`,
which is localed in `<AppName>/app.py`


```python

INSTALLED_APPS = [

    'blog.apps.BlogConfig',
    'users.app.UsersConfig', #
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

use `from django.contrib.auth.forms import UserChangeForm` for a premade registration form

then create a response with templates file in views.py

```python

from django.contrib.auth.forms import UserCreationForm
# Create your views here.
def register(request):
    form = UserCreationForm()
    return render(request, 'user/register.html', {'form':form})

```

for templates, we can use base.html still.

```html
{% extends "blog/base.html" %}
{% block content %}    
    <div class="content-section">
        <form method="POST">
            {%% csrf_token %%} <!--compulsory security-->
            <fieldset class="form-group">
                <legend class="border-bottom mb-4">Join Today</legend>

            </fieldset>
            <div class="form-group">
                <button calss="btn btn-outline-info" type="submit">Sign Up</button>
            </div>
        </form>
        <div class="border-top pt-3">
            <small class="text-muted">
                Already have an account? <a herf="#" class="ml-2">Sign in</a>
            </small>
            
        </div>
    </div>
{% endblock content %}
```

save everything by `form.save()`


use `from django.contrib import messages` can show flash messages on the page.
`messages.debug/info/success/warning/error`


And use `redirect` (also from shortcut) `return redirect('blog-home')`

After redirect, we can show a message on the top of the blogs
```html
{% if messages %}
      {% for message in messages %}
      <div class="alert alert-{{  message.tags }}">
        {{ message }}
      </div>
      {% endfor %}
      {% endif %}
```


### 2.1.2 Create customized form

In `form.py`:

```python

from django import forms
from django.contrib.auth.models import User
from django.contrib.auth.forms import UserCreationForm


class UserRegisterForm(UserCreationForm):
    email = forms.EmailField()

    class Meta:
        model = User
        fields = ['username', 'email', 'password1', 'password2']

```

Now we added email as our request, and just use the User model in the lib.

-----

To use a third-party nice form lib, install `pip install django-crispy-forms`,
then include it in `INSTALLED_APPS` in settings.py: `'crispy-forms'`,
and add `CRISPY_TEMPLATE_PACK = 'bootstrap4'`.

Now add in template: ` {% load crispy_forms_tags %} ` and `{{ form|crispy }}`,

Then it will give a styling.