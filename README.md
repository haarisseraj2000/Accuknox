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

### Question 3: By default do django signals run in the same database transaction as the caller? Please support your answer with a code snippet that conclusively proves your stance. The code does not need to be elegant and production ready, we just need to understand your logic.

#### Ans 3: Yes, by default, Django signals run in the same database transaction as the caller. If the transaction fails or is rolled back, the signal handler’s effects are also rolled back.

`code snippet`
```
from django.db import transaction
from django.db.models.signals import post_save
from django.dispatch import receiver
from django.contrib.auth.models import User

@receiver(post_save, sender=User)
def my_handler(sender, instance, created, **kwargs):
    print("Signal handler executed")
    # Simulate an action in the same transaction
    instance.username = "updated_name"
    instance.save()


try:
    with transaction.atomic():
        print("Creating user")
        user = User.objects.create(username="testuser")
        raise Exception("Forcing rollback")
except Exception as e:
    print(f"Transaction rolled back: {e}")


print("User count:", User.objects.count())

```

`output`
```
Creating user
Signal handler executed
Transaction rolled back: Forcing rollback
User count: 0
```

# Topic: Custom Classes in Python

## Description: You are tasked with creating a Rectangle class with the following requirements:
### 1 .An instance of the Rectangle class requires length:int and width:int to be initialized.
### 2. We can iterate over an instance of the Rectangle class 
### 3. When an instance of the Rectangle class is iterated over, we first get its length in the format: {'length': <VALUE_OF_LENGTH>} followed by the width {width: <VALUE_OF_WIDTH>}

#### Ans: 
` Code`
```
class Rectangle:
    def __init__(self, length: int, width: int):
        self.length = length
        self.width = width
    
    def __iter__(self):
        # Yield length first, then width
        yield {'length': self.length}
        yield {'width': self.width}


rect = Rectangle(5, 3)
for dimension in rect:
    print(dimension)

```

`Output`
```
{'length': 5}
{'width': 3}
```



 
    
