---
title: "Python Notes"
date: 2019-07-01T21:46:38-04:00
draft: true
---

Here's a collection of notes that I believe it's worth sharing in Python.

* [privatemethod](#privatemethod)


### <a id="privatemethod"></a> Private Methods with Underscore

You can define a private method in Python by specifying the underscore `_` which is considered a general convention in the Python code for defining private variables. As an example, we can create a file `httpcall.py` and use the following code:

{{< highlight python >}}
def func():
    return "datacamp"

def _private_func():
    return 7
{{< /highlight >}}

```python
def func():
    return "datacamp"

def _private_func():
    return 7
```