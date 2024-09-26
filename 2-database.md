# Django Tutorial 2 - Database and Migration

## 2.1 Database

> Migration: to apply your changes to the models to the whole architecture.
using `python manage.py makemigrations`
- `python manage.py makemigrations` Create
- `python manage.py migrate` Apply
- `sqlmigrate` **Show SQL Queries** `python manage.py sqlmigrate blog 0001`
- `showmigrations`

------


`models.py` defines data models and database access.

Django uses a **Object-Orientated** way to manipulate the database, so we dont need to change the code when replacing the underlying database.

```python
from django.db import models
from django.utils import timezone
from django.contrib.auth.models import User
# Create your models here.

class Post(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()    #Unrestricted Text
    date_posted = models.DateTimeField(default=timezone.now)
    author = models.ForeignKey(User,on_delete=models.CASCADE) 
```
if the key is deleted by the foreign table
- CASCADE : delete this row as well
- Others: SET_NULL: set the key to null, need manual repair
- DO_NOTHING: do nothing, but may leave problems
- PROTECT: stop the key from being deleted


using `python manage.py makemigrations`

It will make the following changes
![](img/image-3.png)

Then check the SQL codes **Show SQL Queries** `python manage.py sqlmigrate blog 0001`

```SQL
CREATE TABLE "blog_post" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "title" varchar(100) NOT NULL, "content" text NOT NULL, "date_posted" datetime NOT NULL, "author_id" integer NOT NULL REFERENCES "auth_user" ("id") DEFERRABLE INITIALLY DEFERRED);

CREATE INDEX "blog_post_author_id_dd7a8485" ON "blog_post" ("author_id");

COMMIT;
```

Then we apply the changes by `migrate`