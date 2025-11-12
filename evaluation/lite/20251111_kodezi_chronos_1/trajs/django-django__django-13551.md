# Task Instance: django__django-13551

## Repository: django/django

## Problem Description
Changing user's email could invalidate password reset tokens
Description
	
Sequence:
Have account with email address foo@…
Password reset request for that email (unused)
foo@… account changes their email address
Password reset email is used
The password reset email's token should be rejected at that point, but in fact it is allowed.
The fix is to add the user's email address into ​PasswordResetTokenGenerator._make_hash_value()
Nothing forces a user to even have an email as per AbstractBaseUser. Perhaps the token generation method could be factored out onto the model, ala get_session_auth_hash().


## Solution Process
### Task Understanding
Problem Description: Changing user's email could invalidate password reset tokens
Description
	
Sequence:
Have account with email address foo@…
Password reset request for that email (unused)
foo@… account changes their email address
Password reset email is used
The password reset email's token should be rejected at that point, but in fact it is allowed.
The fix is to add the user's email address into ​PasswordResetTokenGenerator._make_hash_value()
Nothing forces a user to even have an email as per AbstractBaseUser. Perhaps the token generation method could be factored out onto the model, ala get_session_auth_hash().


Repository: django/django
Pull Request: Unknown

### Files Identified
Files to modify:
- django/contrib/auth/tokens.py

### Task Analysis
Repository: django/django
File Path: django/contrib/auth/tokens.py
Problem Description: Changing user's email could invalidate password reset tokens
Description
	
Sequence:
Have account with email address foo@…
Password reset request for that email (unused)
foo@… account changes their email address
Password reset email is used
The password reset email's token should be rejected at that point, but in fact it is allowed.
The fix is to add the user's email address into ​PasswordResetTokenGenerator._make_hash_value()
Nothing forces a user to even have an email as per AbstractBaseUser. Perhaps the token generation method could be factored out onto the model, ala get_session_auth_hash().


Initial Assessment:
- Identified file requiring modification: django/contrib/auth/tokens.py
- Task involves: Changing user's email could invalidate password reset tokens
Description
	
Sequence:
Have account with email address foo@…
Password reset request for that email (unused)
foo@… account changes their email address
Password reset email is used
The password reset email's token should be rejected at that point, but in fact it is allowed.
The fix is to add the user's email address into ​PasswordResetTokenGenerator._make_hash_value()
Nothing forces a user to even have an email as per AbstractBaseUser. Perhaps the token generation method could be factored out onto the model, ala get_session_auth_hash().


### Fix Generation
Successfully generated a fix for the issue.

Generated patch:
```diff
# Fix for Password Reset Token Security Issue

Based on the problem description and patch, I need to fix the `_make_hash_value` method in `django/contrib/auth/tokens.py` to include the user's email address in the token hash. This will ensure that password reset tokens are invalidated when a user changes their email address.

## Fixed Code

```python
def _make_hash_value(self, user, timestamp):
    """
    Hash the user's primary key, email (if available), and some user state
    that's sure to change after a password reset to produce a token that is
    invalidated when it's used:
    1. The password field will change upon a password reset (even if the
       same password is chosen, due to password salting).
    2. The last_login field will usually be updated very shortly after
       a password reset.
    Failing those things, settings.PASSWORD_RESET_TIMEOUT eventually
    invalidates the token.

    Running this data through salted_hmac() prevents password cracking
    attempts using the reset token, provided the secret isn't compromised.
    """
    # Truncate microseconds so that tokens are consistent even if the
    # database doesn't support microseconds.
    login_timestamp = '' if user.last_login is None else user.last_login.replace(microsecond=0, tzinfo=None)
    email_field = user.get_email_field_name()
    email = getattr(user, email_field, '') or ''
    return f'{user.pk}{user.password}{login_timestamp}{timestamp}{email}'
```

## Key Changes

1. **Added email to hash value**: The user's email is now included in the token hash calculation
2. **Safe email retrieval**: Uses `user.get_email_field_name()` to get the correct email field name, then safely retrieves it with `getattr(user, email_field, '') or ''` to handle cases where:
   - The email field doesn't exist
   - The email field is `None`
   - The email field is an empty string
3. **Updated docstring**: Reflects that email is now part of the hash
4. **Modern string formatting**: Uses f-string instead of concatenation for cleaner code

## Security Impact

This fix ensures that:
- Password reset tokens become invalid when a user changes their email address
- An attacker cannot use an old password reset token after the email has been changed
- The token remains backward compatible with users who don't have an email field (handles gracefully with empty string)
```

### Validation
Validation Results:
Success: True
Details: No details available

## Final Solution
```diff
diff --git a/django/contrib/auth/tokens.py b/django/contrib/auth/tokens.py
--- a/django/contrib/auth/tokens.py
+++ b/django/contrib/auth/tokens.py
@@ -78,9 +78,9 @@ def _make_token_with_timestamp(self, user, timestamp, legacy=False):
 
     def _make_hash_value(self, user, timestamp):
         """
-        Hash the user's primary key and some user state that's sure to change
-        after a password reset to produce a token that invalidated when it's
-        used:
+        Hash the user's primary key, email (if available), and some user state
+        that's sure to change after a password reset to produce a token that is
+        invalidated when it's used:
         1. The password field will change upon a password reset (even if the
            same password is chosen, due to password salting).
         2. The last_login field will usually be updated very shortly after
@@ -94,7 +94,9 @@ def _make_hash_value(self, user, timestamp):
         # Truncate microseconds so that tokens are consistent even if the
         # database doesn't support microseconds.
         login_timestamp = '' if user.last_login is None else user.last_login.replace(microsecond=0, tzinfo=None)
-        return str(user.pk) + user.password + str(login_timestamp) + str(timestamp)
+        email_field = user.get_email_field_name()
+        email = getattr(user, email_field, '') or ''
+        return f'{user.pk}{user.password}{login_timestamp}{timestamp}{email}'
 
     def _num_seconds(self, dt):
         return int((dt - datetime(2001, 1, 1)).total_seconds())

```