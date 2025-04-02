# Accuknox
## Assignment

### Question 1: By default are django signals executed synchronously or asynchronously? Please support your answer with a code snippet that conclusively proves your stance. The code does not need to be elegant and production ready, we just need to understand your logic.

# Ans 1: By default, Django signals are executed synchronously.
# code snippet:
 Inline `code`
from django.db.models.signals import post_save

from django.dispatch import receiver

from django.contrib.auth.models import User

import time

@receiver(post_save, sender=User)
def my_handler(sender, instance, created, **kwargs):
    print("Signal handler started")
    time.sleep(2)  
    print("Signal handler finished")
 
    
