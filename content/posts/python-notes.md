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

