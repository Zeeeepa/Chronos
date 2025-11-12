# Task Instance: scikit-learn__scikit-learn-14983

## Repository: scikit-learn/scikit-learn

## Problem Description
RepeatedKFold and RepeatedStratifiedKFold do not show correct __repr__ string
#### Description

`RepeatedKFold` and `RepeatedStratifiedKFold` do not show correct \_\_repr\_\_ string.

#### Steps/Code to Reproduce

```python
>>> from sklearn.model_selection import RepeatedKFold, RepeatedStratifiedKFold
>>> repr(RepeatedKFold())
>>> repr(RepeatedStratifiedKFold())
```

#### Expected Results

```python
>>> repr(RepeatedKFold())
RepeatedKFold(n_splits=5, n_repeats=10, random_state=None)
>>> repr(RepeatedStratifiedKFold())
RepeatedStratifiedKFold(n_splits=5, n_repeats=10, random_state=None)
```

#### Actual Results

```python
>>> repr(RepeatedKFold())
'<sklearn.model_selection._split.RepeatedKFold object at 0x0000016421AA4288>'
>>> repr(RepeatedStratifiedKFold())
'<sklearn.model_selection._split.RepeatedStratifiedKFold object at 0x0000016420E115C8>'
```

#### Versions
```
System:
    python: 3.7.4 (default, Aug  9 2019, 18:34:13) [MSC v.1915 64 bit (AMD64)]
executable: D:\anaconda3\envs\xyz\python.exe
   machine: Windows-10-10.0.16299-SP0

BLAS:
    macros:
  lib_dirs:
cblas_libs: cblas

Python deps:
       pip: 19.2.2
setuptools: 41.0.1
   sklearn: 0.21.2
     numpy: 1.16.4
     scipy: 1.3.1
    Cython: None
    pandas: 0.24.2
```


## Solution Process
### Task Understanding
Problem Description: RepeatedKFold and RepeatedStratifiedKFold do not show correct __repr__ string
#### Description

`RepeatedKFold` and `RepeatedStratifiedKFold` do not show correct \_\_repr\_\_ string.

#### Steps/Code to Reproduce

```python
>>> from sklearn.model_selection import RepeatedKFold, RepeatedStratifiedKFold
>>> repr(RepeatedKFold())
>>> repr(RepeatedStratifiedKFold())
```

#### Expected Results

```python
>>> repr(RepeatedKFold())
RepeatedKFold(n_splits=5, n_repeats=10, random_state=None)
>>> repr(RepeatedStratifiedKFold())
RepeatedStratifiedKFold(n_splits=5, n_repeats=10, random_state=None)
```

#### Actual Results

```python
>>> repr(RepeatedKFold())
'<sklearn.model_selection._split.RepeatedKFold object at 0x0000016421AA4288>'
>>> repr(RepeatedStratifiedKFold())
'<sklearn.model_selection._split.RepeatedStratifiedKFold object at 0x0000016420E115C8>'
```

#### Versions
```
System:
    python: 3.7.4 (default, Aug  9 2019, 18:34:13) [MSC v.1915 64 bit (AMD64)]
executable: D:\anaconda3\envs\xyz\python.exe
   machine: Windows-10-10.0.16299-SP0

BLAS:
    macros:
  lib_dirs:
cblas_libs: cblas

Python deps:
       pip: 19.2.2
setuptools: 41.0.1
   sklearn: 0.21.2
     numpy: 1.16.4
     scipy: 1.3.1
    Cython: None
    pandas: 0.24.2
```


Repository: scikit-learn/scikit-learn
Pull Request: Unknown

### Files Identified
Files to modify:
- sklearn/model_selection/_split.py

### Task Analysis
Repository: scikit-learn/scikit-learn
File Path: sklearn/model_selection/_split.py
Problem Description: RepeatedKFold and RepeatedStratifiedKFold do not show correct __repr__ string
#### Description

`RepeatedKFold` and `RepeatedStratifiedKFold` do not show correct \_\_repr\_\_ string.

#### Steps/Code to Reproduce

```python
>>> from sklearn.model_selection import RepeatedKFold, RepeatedStratifiedKFold
>>> repr(RepeatedKFold())
>>> repr(RepeatedStratifiedKFold())
```

#### Expected Results

```python
>>> repr(RepeatedKFold())
RepeatedKFold(n_splits=5, n_repeats=10, random_state=None)
>>> repr(RepeatedStratifiedKFold())
RepeatedStratifiedKFold(n_splits=5, n_repeats=10, random_state=None)
```

#### Actual Results

```python
>>> repr(RepeatedKFold())
'<sklearn.model_selection._split.RepeatedKFold object at 0x0000016421AA4288>'
>>> repr(RepeatedStratifiedKFold())
'<sklearn.model_selection._split.RepeatedStratifiedKFold object at 0x0000016420E115C8>'
```

#### Versions
```
System:
    python: 3.7.4 (default, Aug  9 2019, 18:34:13) [MSC v.1915 64 bit (AMD64)]
executable: D:\anaconda3\envs\xyz\python.exe
   machine: Windows-10-10.0.16299-SP0

BLAS:
    macros:
  lib_dirs:
cblas_libs: cblas

Python deps:
       pip: 19.2.2
setuptools: 41.0.1
   sklearn: 0.21.2
     numpy: 1.16.4
     scipy: 1.3.1
    Cython: None
    pandas: 0.24.2
```


Initial Assessment:
- Identified file requiring modification: sklearn/model_selection/_split.py
- Task involves: RepeatedKFold and RepeatedStratifiedKFold do not show correct __repr__ string
#### Description

`RepeatedKFold` and `RepeatedStratifiedKFold` do not show correct \_\_repr\_\_ string.

#### Steps/Code to Reproduce

```python
>>> from sklearn.model_selection import RepeatedKFold, RepeatedStratifiedKFold
>>> repr(RepeatedKFold())
>>> repr(RepeatedStratifiedKFold())
```

#### Expected Results

```python
>>> repr(RepeatedKFold())
RepeatedKFold(n_splits=5, n_repeats=10, random_state=None)
>>> repr(RepeatedStratifiedKFold())
RepeatedStratifiedKFold(n_splits=5, n_repeats=10, random_state=None)
```

#### Actual Results

```python
>>> repr(RepeatedKFold())
'<sklearn.model_selection._split.RepeatedKFold object at 0x0000016421AA4288>'
>>> repr(RepeatedStratifiedKFold())
'<sklearn.model_selection._split.RepeatedStratifiedKFold object at 0x0000016420E115C8>'
```

#### Versions
```
System:
    python: 3.7.4 (default, Aug  9 2019, 18:34:13) [MSC v.1915 64 bit (AMD64)]
executable: D:\anaconda3\envs\xyz\python.exe
   machine: Windows-10-10.0.16299-SP0

BLAS:
    macros:
  lib_dirs:
cblas_libs: cblas

Python deps:
       pip: 19.2.2
setuptools: 41.0.1
   sklearn: 0.21.2
     numpy: 1.16.4
     scipy: 1.3.1
    Cython: None
    pandas: 0.24.2
```


### Fix Generation
Successfully generated a fix for the issue.

Generated patch:
```diff
Looking at the problem, I need to:

1. Add a `__repr__` method to the `_RepeatedSplits` base class
2. Modify the `_build_repr` function to handle the `cvargs` attribute that stores parameters for repeated splits

Here's the fix:

```python
# In the _RepeatedSplits class (around line 1163), add the __repr__ method:

class _RepeatedSplits(metaclass=ABCMeta):
    """Base class for repeated cross-validators."""
    
    # ... existing code ...
    
    def get_n_splits(self, X=None, y=None, groups=None):
        """Returns the number of splitting iterations in the cross-validator
        
        Parameters
        ----------
        X : object
            Always ignored, exists for compatibility.
            
        y : object
            Always ignored, exists for compatibility.
            
        groups : object
            Always ignored, exists for compatibility.
            
        Returns
        -------
        n_splits : int
            Returns the number of splitting iterations in the cross-validator.
        """
        rng = check_random_state(self.random_state)
        cv = self.cv(random_state=rng, **self.cvargs)
        return cv.get_n_splits(X, y, groups) * self.n_repeats
    
    def __repr__(self):
        return _build_repr(self)


# In the _build_repr function (around line 2158), modify it to handle cvargs:

def _build_repr(self):
    # XXX This is copied from BaseEstimator's get_params
    cls = self.__class__
    init = getattr(cls.__init__, 'deprecated_original', cls.__init__)
    # Ignore varargs, kw and default values and pop self
    init_signature = signature(init)
    # Consider the constructor parameters excluding 'self'
    if init is object.__init__:
        args = []
    else:
        args = sorted([p.name for p in init_signature.parameters.values()
                       if p.name != 'self' and p.kind != p.VAR_KEYWORD])
    class_name = self.__class__.__name__
    params = dict()
    for key in args:
        # We need deprecation warnings to always be on in order to
        # catch deprecated param values.
        # This is set in utils/__init__.py but it gets overwritten
        # when running under python3 somehow.
        warnings.simplefilter("always", DeprecationWarning)
        try:
            with warnings.catch_warnings(record=True) as w:
                value = getattr(self, key, None)
                if value is None and hasattr(self, 'cvargs'):
                    value = self.cvargs.get(key, None)
            if len(w) and w[0].category == DeprecationWarning:
                # if the parameter is deprecated, don't show it
                continue
        finally:
            warnings.filters.pop(0)
        params[key] = value

    return '%s(%s)' % (class_name, _pprint(params, offset=len(class_name)))
```

**Explanation of the fix:**

1. **Added `__repr__` method to `_RepeatedSplits`**: This ensures that both `RepeatedKFold` and `RepeatedStratifiedKFold` (which inherit from `_RepeatedSplits`) will use the `_build_repr` function to generate their string representation.

2. **Modified `_build_repr` to check `cvargs`**: The `_RepeatedSplits` class stores parameters like `n_splits` in a `cvargs` dictionary that gets passed to the underlying cross-validator. The fix checks if an attribute value is `None` and if the object has a `cvargs` attribute, it retrieves the value from there instead.

This ensures that when `repr()` is called on `RepeatedKFold()` or `RepeatedStratifiedKFold()`, it will properly display:
- `RepeatedKFold(n_splits=5, n_repeats=10, random_state=None)`
- `RepeatedStratifiedKFold(n_splits=5, n_repeats=10, random_state=None)`

Instead of the default object representation.
```

### Validation
Validation Results:
Success: True
Details: No details available

## Final Solution
```diff
diff --git a/sklearn/model_selection/_split.py b/sklearn/model_selection/_split.py
--- a/sklearn/model_selection/_split.py
+++ b/sklearn/model_selection/_split.py
@@ -1163,6 +1163,9 @@ def get_n_splits(self, X=None, y=None, groups=None):
                      **self.cvargs)
         return cv.get_n_splits(X, y, groups) * self.n_repeats
 
+    def __repr__(self):
+        return _build_repr(self)
+
 
 class RepeatedKFold(_RepeatedSplits):
     """Repeated K-Fold cross validator.
@@ -2158,6 +2161,8 @@ def _build_repr(self):
         try:
             with warnings.catch_warnings(record=True) as w:
                 value = getattr(self, key, None)
+                if value is None and hasattr(self, 'cvargs'):
+                    value = self.cvargs.get(key, None)
             if len(w) and w[0].category == DeprecationWarning:
                 # if the parameter is deprecated, don't show it
                 continue

```