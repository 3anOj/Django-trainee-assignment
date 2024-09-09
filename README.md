# Django-trainee-assignment
# Question 1

from django.db import models
from django.db.models.signals import post_save
from django.dispatch import receiver
import time

class MyModel(models.Model):
    name = models.CharField(max_length=100)

@receiver(post_save, sender=MyModel)
def my_signal_handler(sender, instance, created, **kwargs):
    print(f"Signal handler started for {instance.name}")
    time.sleep(2)  # Simulate some work
    print(f"Signal handler finished for {instance.name}")


instance = MyModel(name="Test")
print("Saving instance...")
instance.save()
print("Instance saved.")

# Question 2

import threading
from django.db import models
from django.db.models.signals import post_save
from django.dispatch import receiver

class MyModel(models.Model):
    name = models.CharField(max_length=100)

@receiver(post_save, sender=MyModel)
def my_signal_handler(sender, instance, created, **kwargs):
    current_thread = threading.current_thread()
    print(f"Signal handler running in thread: {current_thread.name}")

def create_model_instance():
    current_thread = threading.current_thread()
    print(f"Creating instance in thread: {current_thread.name}")
    instance = MyModel(name="Test")
    instance.save()

# Question 3

from django.db import models, transaction
from django.db.models.signals import post_save
from django.dispatch import receiver

class MyModel(models.Model):
    name = models.CharField(max_length=100)

@receiver(post_save, sender=MyModel)
def my_signal_handler(sender, instance, created, **kwargs):
    print(f"Signal handler: Updating {instance.name}")
    instance.name = f"{instance.name} (updated in signal)"
    instance.save()

def create_model_instance():
    with transaction.atomic():
        instance = MyModel(name="Original")
        print("Saving instance...")
        instance.save()
        print(f"Instance name after first save: {instance.name}")
        
        instance.refresh_from_db()
        print(f"Instance name after refresh: {instance.name}")

# Custom class Python

class Rectangle:
    def __init__(self, length: int, width: int):
        self.length = length
        self.width = width
    
    def __iter__(self):
        yield {'length': self.length}
        yield {'width': self.width}

rect = Rectangle(5, 3)
for item in rect:
    print(item)


