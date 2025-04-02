# Accuknox
## Assignment

### Question 1: By default are django signals executed synchronously or asynchronously? Please support your answer with a code snippet that conclusively proves your stance. The code does not need to be elegant and production ready, we just need to understand your logic.

#### Ans 1: By default, Django signals are executed synchronously.
##### code snippet:
 `code`
``` 
from django.db.models.signals import post_save

from django.dispatch import receiver

from django.contrib.auth.models import User

import time

@receiver(post_save, sender=User)
def my_handler(sender, instance, created, **kwargs):
    print("Signal handler started")
    time.sleep(2)  
    print("Signal handler finished")

```

### Question 2: Do django signals run in the same thread as the caller? Please support your answer with a code snippet that conclusively proves your stance. The code does not need to be elegant and production ready, we just need to understand your logic.

#### Ans2 : Yes, by default, Django signals run in the same thread as the caller, as they are executed synchronously in the same process.

##### code snippet:

`code snippet`

```
from django.db.models.signals import post_save
from django.dispatch import receiver
from django.contrib.auth.models import User
import threading

@receiver(post_save, sender=User)
def my_handler(sender, instance, created, **kwargs):
    print(f"Signal handler thread: {threading.current_thread().name}")


print(f"Caller thread: {threading.current_thread().name}")
user = User.objects.create(username="testuser")

```

`Output`
```
Caller thread: MainThread
Signal handler thread: MainThread

```

 
    
