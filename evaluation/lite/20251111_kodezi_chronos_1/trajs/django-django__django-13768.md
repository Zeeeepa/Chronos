# Task Instance: django__django-13768

## Repository: django/django

## Problem Description
Log exceptions handled in Signal.send_robust()
Description
	
As pointed out by ​Haki Benita on Twitter, by default Signal.send_robust() doesn't have any log messages for exceptions raised in receivers. Since Django logs exceptions in other similar situations, such as missing template variables, I think it would be worth adding a logger.exception() call in the except clause of send_robust() . Users would then see such exceptions in their error handling tools, e.g. Sentry, and be able to figure out what action to take from there. Ultimately any *expected* exception should be caught with a try in the receiver function.


## Solution Process
### Task Understanding
Problem Description: Log exceptions handled in Signal.send_robust()
Description
	
As pointed out by ​Haki Benita on Twitter, by default Signal.send_robust() doesn't have any log messages for exceptions raised in receivers. Since Django logs exceptions in other similar situations, such as missing template variables, I think it would be worth adding a logger.exception() call in the except clause of send_robust() . Users would then see such exceptions in their error handling tools, e.g. Sentry, and be able to figure out what action to take from there. Ultimately any *expected* exception should be caught with a try in the receiver function.


Repository: django/django
Pull Request: Unknown

### Files Identified
Files to modify:
- django/dispatch/dispatcher.py

### Task Analysis
Repository: django/django
File Path: django/dispatch/dispatcher.py
Problem Description: Log exceptions handled in Signal.send_robust()
Description
	
As pointed out by ​Haki Benita on Twitter, by default Signal.send_robust() doesn't have any log messages for exceptions raised in receivers. Since Django logs exceptions in other similar situations, such as missing template variables, I think it would be worth adding a logger.exception() call in the except clause of send_robust() . Users would then see such exceptions in their error handling tools, e.g. Sentry, and be able to figure out what action to take from there. Ultimately any *expected* exception should be caught with a try in the receiver function.


Initial Assessment:
- Identified file requiring modification: django/dispatch/dispatcher.py
- Task involves: Log exceptions handled in Signal.send_robust()
Description
	
As pointed out by ​Haki Benita on Twitter, by default Signal.send_robust() doesn't have any log messages for exceptions raised in receivers. Since Django logs exceptions in other similar situations, such as missing template variables, I think it would be worth adding a logger.exception() call in the except clause of send_robust() . Users would then see such exceptions in their error handling tools, e.g. Sentry, and be able to figure out what action to take from there. Ultimately any *expected* exception should be caught with a try in the receiver function.


### Fix Generation
Successfully generated a fix for the issue.

Generated patch:
```diff
I'll analyze the problem and generate a fix for adding logging to `Signal.send_robust()` in Django's dispatch system.

## Analysis

The issue is that `Signal.send_robust()` silently catches exceptions from signal receivers without logging them. This makes debugging difficult since developers won't see these exceptions in their error monitoring tools.

## Solution

Here's the fix to add exception logging to the `send_robust()` method:

```python
# django/dispatch/dispatcher.py

import logging
import threading
import warnings
import weakref

from django.utils.deprecation import RemovedInDjango40Warning
from django.utils.inspect import func_accepts_kwargs

logger = logging.getLogger('django.dispatch')


def _make_id(target):
    if hasattr(target, '__func__'):
        return (id(target.__self__), id(target.__func__))
    return id(target)


# ... (other code remains the same)


class Signal:
    # ... (other methods remain the same)
    
    def send_robust(self, sender, **named):
        """
        Send signal from sender to all connected receivers catching errors.

        Arguments:

            sender
                The sender of the signal. Can be any Python object (normally one
                registered with a connect if you actually want something to
                occur).

            named
                Named arguments which will be passed to receivers.

        Return a list of tuple pairs [(receiver, response), ... ].

        If any receiver raises an error (specifically any subclass of
        Exception), return the error instance as the result for that receiver.
        """
        if not self.receivers or self.sender_receivers_cache.get(sender) is NO_RECEIVERS:
            return []

        # Call each receiver with whatever arguments it can accept.
        # Return a list of tuple pairs [(receiver, response), ... ].
        responses = []
        for receiver in self._live_receivers(sender):
            try:
                response = receiver(signal=self, sender=sender, **named)
            except Exception as err:
                logger.error(
                    'Error calling %s in Signal.send_robust() (%s)',
                    receiver.__qualname__,
                    err,
                    exc_info=err,
                )
                responses.append((receiver, err))
            else:
                responses.append((receiver, response))
        return responses
```

## Key Changes

1. **Added `import logging`** at the top of the file
2. **Created a logger instance** with `logger = logging.getLogger('django.dispatch')`
3. **Added logging in the exception handler** within `send_robust()`:
   - Uses `logger.error()` to log at ERROR level
   - Includes the receiver's qualified name (`receiver.__qualname__`)
   - Includes the exception message
   - Passes `exc_info=err` to include the full traceback

## Benefits

- Exceptions are now logged and will appear in error monitoring tools (Sentry, etc.)
- Developers can identify which receiver is causing problems
- Full stack traces are available for debugging
- The original behavior is preserved (exceptions are still caught and returned in the response list)
- Uses Django's standard logging approach consistent with other parts of the framework

This fix ensures that unexpected exceptions in signal receivers are visible to developers while maintaining the robust error-handling behavior of `send_robust()`.
```

### Validation
Validation Results:
Success: True
Details: No details available

## Final Solution
```diff
diff --git a/django/dispatch/dispatcher.py b/django/dispatch/dispatcher.py
--- a/django/dispatch/dispatcher.py
+++ b/django/dispatch/dispatcher.py
@@ -1,3 +1,4 @@
+import logging
 import threading
 import warnings
 import weakref
@@ -5,6 +6,8 @@
 from django.utils.deprecation import RemovedInDjango40Warning
 from django.utils.inspect import func_accepts_kwargs
 
+logger = logging.getLogger('django.dispatch')
+
 
 def _make_id(target):
     if hasattr(target, '__func__'):
@@ -208,6 +211,12 @@ def send_robust(self, sender, **named):
             try:
                 response = receiver(signal=self, sender=sender, **named)
             except Exception as err:
+                logger.error(
+                    'Error calling %s in Signal.send_robust() (%s)',
+                    receiver.__qualname__,
+                    err,
+                    exc_info=err,
+                )
                 responses.append((receiver, err))
             else:
                 responses.append((receiver, response))

```