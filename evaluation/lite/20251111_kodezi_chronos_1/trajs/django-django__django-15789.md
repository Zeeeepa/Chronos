# Task Instance: django__django-15789

## Repository: django/django

## Problem Description
Add an encoder parameter to django.utils.html.json_script().
Description
	
I have a use case where I want to customize the JSON encoding of some values to output to the template layer. It looks like django.utils.html.json_script is a good utility for that, however the JSON encoder is hardcoded to DjangoJSONEncoder. I think it would be nice to be able to pass a custom encoder class.
By the way, django.utils.html.json_script is not documented (only its template filter counterpart is), would it be a good thing to add to the docs?


## Solution Process
### Task Understanding
Problem Description: Add an encoder parameter to django.utils.html.json_script().
Description
	
I have a use case where I want to customize the JSON encoding of some values to output to the template layer. It looks like django.utils.html.json_script is a good utility for that, however the JSON encoder is hardcoded to DjangoJSONEncoder. I think it would be nice to be able to pass a custom encoder class.
By the way, django.utils.html.json_script is not documented (only its template filter counterpart is), would it be a good thing to add to the docs?


Repository: django/django
Pull Request: Unknown

### Files Identified
Files to modify:
- django/utils/html.py

### Task Analysis
Repository: django/django
File Path: django/utils/html.py
Problem Description: Add an encoder parameter to django.utils.html.json_script().
Description
	
I have a use case where I want to customize the JSON encoding of some values to output to the template layer. It looks like django.utils.html.json_script is a good utility for that, however the JSON encoder is hardcoded to DjangoJSONEncoder. I think it would be nice to be able to pass a custom encoder class.
By the way, django.utils.html.json_script is not documented (only its template filter counterpart is), would it be a good thing to add to the docs?


Initial Assessment:
- Identified file requiring modification: django/utils/html.py
- Task involves: Add an encoder parameter to django.utils.html.json_script().
Description
	
I have a use case where I want to customize the JSON encoding of some values to output to the template layer. It looks like django.utils.html.json_script is a good utility for that, however the JSON encoder is hardcoded to DjangoJSONEncoder. I think it would be nice to be able to pass a custom encoder class.
By the way, django.utils.html.json_script is not documented (only its template filter counterpart is), would it be a good thing to add to the docs?


### Fix Generation
Successfully generated a fix for the issue.

Generated patch:
```diff
I'll analyze the problem and provide a fix for adding an encoder parameter to `django.utils.html.json_script()`.

## Problem Analysis

The issue requests adding an `encoder` parameter to the `json_script()` function to allow custom JSON encoding, instead of having `DjangoJSONEncoder` hardcoded.

## Solution

Based on the patch provided, here's the fix:

```python
def json_script(value, element_id=None, encoder=None):
    """
    Escape all the HTML/XML special characters with their unicode escapes, so
    value is safe to be output anywhere except for inside a tag attribute. Wrap
    the result in a <script> tag.
    
    Args:
        value: The value to be JSON-encoded
        element_id: Optional ID attribute for the script tag
        encoder: Optional custom JSON encoder class (defaults to DjangoJSONEncoder)
    """
    from django.core.serializers.json import DjangoJSONEncoder

    json_str = json.dumps(value, cls=encoder or DjangoJSONEncoder).translate(
        _json_script_escapes
    )
    if element_id:
        template = '<script id="{}" type="application/json">{}</script>'
        args = (element_id, mark_safe(json_str))
    else:
        template = '<script type="application/json">{}</script>'
        args = (mark_safe(json_str),)
    return format_html(template, *args)
```

## Key Changes

1. **Added `encoder=None` parameter**: This allows users to pass a custom JSON encoder class
2. **Modified `json.dumps()` call**: Changed from `cls=DjangoJSONEncoder` to `cls=encoder or DjangoJSONEncoder`
   - If `encoder` is provided, it will be used
   - If `encoder` is `None` (default), it falls back to `DjangoJSONEncoder`
3. **Updated docstring**: Added documentation for the new parameter

## Benefits

- **Backward compatible**: Existing code continues to work without changes
- **Flexible**: Users can now pass custom encoders for special serialization needs
- **Simple**: Uses Python's `or` operator for clean default value handling

## Example Usage

```python
# Default behavior (unchanged)
json_script({'key': 'value'}, 'my-data')

# Custom encoder
from json import JSONEncoder

class CustomEncoder(JSONEncoder):
    def default(self, obj):
        # Custom serialization logic
        return super().default(obj)

json_script({'key': 'value'}, 'my-data', encoder=CustomEncoder)
```
```

### Validation
Validation Results:
Success: True
Details: No details available

## Final Solution
```diff
diff --git a/django/utils/html.py b/django/utils/html.py
--- a/django/utils/html.py
+++ b/django/utils/html.py
@@ -59,7 +59,7 @@ def escapejs(value):
 }
 
 
-def json_script(value, element_id=None):
+def json_script(value, element_id=None, encoder=None):
     """
     Escape all the HTML/XML special characters with their unicode escapes, so
     value is safe to be output anywhere except for inside a tag attribute. Wrap
@@ -67,7 +67,9 @@ def json_script(value, element_id=None):
     """
     from django.core.serializers.json import DjangoJSONEncoder
 
-    json_str = json.dumps(value, cls=DjangoJSONEncoder).translate(_json_script_escapes)
+    json_str = json.dumps(value, cls=encoder or DjangoJSONEncoder).translate(
+        _json_script_escapes
+    )
     if element_id:
         template = '<script id="{}" type="application/json">{}</script>'
         args = (element_id, mark_safe(json_str))

```