---
title: "Python Notes"
date: 2019-07-01T21:46:38-04:00
draft: true
---

Here's a collection of notes that I believe it's worth sharing in Python.

* [privatemethod](#privatemethod)


### <a id="privatemethod"></a> Private Methods with Underscore

You can define a private method in Python by specifying the underscore `_` which is considered a general convention in the Python code for defining private variables. As an example, we can create a file `httpcall.py` and use the following code:

`httpcall.py`
```python
import urllib.request
from urllib.parse import urlparse

# private function
def _verify_url(url):
    try:
        result = urlparse(url)
        return all([result.scheme, result.netloc])
    except:
        return False

# public method
def get_http_resource(url):
    if _verify_url(url) == False:
        return "Error parsing the provided URL."
    contents = urllib.request.urlopen(url).read()
    title = str(contents).split('<title>')[1].split('</title>')[0]
    return title
```
Now, let's open Python shell and explore the package:

```python
Python 3.5.0 (default, Jun 29 2017, 22:45:13)
[GCC 4.2.1 Compatible Apple LLVM 8.1.0 (clang-802.0.42)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import httpcall
>>> dir(httpcall)
['__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', '_verify_url', 'get_http_resource', 'urllib', 'urlparse']
>>> from httpcall import *
>>> _verify_url("http://google.com") # method can't be called due to the underscore "private"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name '_verify_url' is not defined
>>> get_http_resource("http://google.com")
'Google'
>>> import httpcall as hc
>>> hc._verify_url("http://google.com") # calling the private method directly is possible using the normal import 
True
```
