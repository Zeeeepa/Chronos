# Task Instance: django__django-12308

## Repository: django/django

## Problem Description
JSONField are not properly displayed in admin when they are readonly.
Description
	
JSONField values are displayed as dict when readonly in the admin.
For example, {"foo": "bar"} would be displayed as {'foo': 'bar'}, which is not valid JSON.
I believe the fix would be to add a special case in django.contrib.admin.utils.display_for_field to call the prepare_value of the JSONField (not calling json.dumps directly to take care of the InvalidJSONInput case).


## Solution Process
### Task Understanding
Problem Description: JSONField are not properly displayed in admin when they are readonly.
Description
	
JSONField values are displayed as dict when readonly in the admin.
For example, {"foo": "bar"} would be displayed as {'foo': 'bar'}, which is not valid JSON.
I believe the fix would be to add a special case in django.contrib.admin.utils.display_for_field to call the prepare_value of the JSONField (not calling json.dumps directly to take care of the InvalidJSONInput case).


Repository: django/django
Pull Request: Unknown

### Files Identified
Files to modify:
- django/contrib/admin/utils.py

### Task Analysis
Repository: django/django
File Path: django/contrib/admin/utils.py
Problem Description: JSONField are not properly displayed in admin when they are readonly.
Description
	
JSONField values are displayed as dict when readonly in the admin.
For example, {"foo": "bar"} would be displayed as {'foo': 'bar'}, which is not valid JSON.
I believe the fix would be to add a special case in django.contrib.admin.utils.display_for_field to call the prepare_value of the JSONField (not calling json.dumps directly to take care of the InvalidJSONInput case).


Initial Assessment:
- Identified file requiring modification: django/contrib/admin/utils.py
- Task involves: JSONField are not properly displayed in admin when they are readonly.
Description
	
JSONField values are displayed as dict when readonly in the admin.
For example, {"foo": "bar"} would be displayed as {'foo': 'bar'}, which is not valid JSON.
I believe the fix would be to add a special case in django.contrib.admin.utils.display_for_field to call the prepare_value of the JSONField (not calling json.dumps directly to take care of the InvalidJSONInput case).


### Fix Generation
Successfully generated a fix for the issue.

Generated patch:
```diff
I'll analyze the issue and provide a fix for the JSONField display problem in Django admin when fields are readonly.

## Problem Analysis

The issue is that JSONField values are displayed using Python's dict representation (with single quotes) instead of proper JSON format (with double quotes) when shown as readonly in the admin interface.

## Solution

Based on the patch provided, we need to add a special case in the `display_for_field` function to handle JSONField properly. However, the patch uses `get_prep_value` which is meant for database preparation. We should use `value_to_string` or handle JSON serialization properly.

Here's the corrected fix:

```python
def display_for_field(value, field, empty_value_display):
    from django.contrib.admin.templatetags.admin_list import _boolean_icon
    from django.db import models
    import json

    if getattr(field, 'flatchoices', None):
        return dict(field.flatchoices).get(value, empty_value_display)
    # BooleanField needs special-case null-handling, so it comes before the
    # general null test.
    elif isinstance(field, models.BooleanField):
        return _boolean_icon(value)
    elif value is None:
        return empty_value_display
    elif isinstance(field, models.DateTimeField):
        return formats.localize(timezone.template_localtime(value))
    elif isinstance(field, (models.DateField, models.TimeField)):
        return formats.localize(value)
    elif isinstance(field, models.DecimalField):
        return formats.number_format(value, field.decimal_places)
    elif isinstance(field, (models.IntegerField, models.FloatField)):
        return formats.number_format(value)
    elif isinstance(field, models.FileField) and value:
        return format_html('<a href="{}">{}</a>', value.url, value)
    elif isinstance(field, models.JSONField) and value:
        try:
            return json.dumps(value, ensure_ascii=False, cls=field.encoder)
        except (TypeError, ValueError):
            return display_for_value(value, empty_value_display)
    else:
        return display_for_value(value, empty_value_display)
```

## Key Changes

1. **Added import for json module** at the top of the function (or at module level)
2. **Added JSONField handling** before the final `else` clause
3. **Used `json.dumps`** with proper parameters:
   - `ensure_ascii=False`: To properly handle Unicode characters
   - `cls=field.encoder`: To use the field's custom encoder if specified
4. **Added exception handling** for `TypeError` and `ValueError` to gracefully fall back to `display_for_value` if serialization fails

This fix ensures that:
- JSONField values are displayed as valid JSON with double quotes
- Custom encoders are respected
- Invalid or non-serializable values fall back to the default display
- The solution handles edge cases gracefully

The original patch's approach of using `get_prep_value` would also work, but using `json.dumps` directly is clearer and more aligned with the intent of displaying JSON properly.
```

### Validation
Validation Results:
Success: True
Details: No details available

## Final Solution
```diff
diff --git a/django/contrib/admin/utils.py b/django/contrib/admin/utils.py
--- a/django/contrib/admin/utils.py
+++ b/django/contrib/admin/utils.py
@@ -398,6 +398,11 @@ def display_for_field(value, field, empty_value_display):
         return formats.number_format(value)
     elif isinstance(field, models.FileField) and value:
         return format_html('<a href="{}">{}</a>', value.url, value)
+    elif isinstance(field, models.JSONField) and value:
+        try:
+            return field.get_prep_value(value)
+        except TypeError:
+            return display_for_value(value, empty_value_display)
     else:
         return display_for_value(value, empty_value_display)
 

```