# Unirest for Python

Unirest is a set of lightweight HTTP libraries in multiple languages.

Created with love by http://mashape.com



## Installing
To utilize unirest, install it using pip:

`pip install unirest`

After installing the pip package, you can now begin simplifying requests by importing unirest:

`import unirest`

### Creating Requests
So you're probably wondering how using Unirest makes creating requests in Python easier, let's start with a working example:

```python
response = unirest.post("http://httpbin.org/post", headers={ "Accept": "application/json" }, params={ "parameter": 23, "foo": "bar" })
```

## Asynchronous Requests
Python also has support for asynchronous requests in which you can define a `callback` to be passed along and invoked when Unirest recieves the response.
For non-blocking requests in Python we need to define ourselves a callback to reference inside of our request method upon response:

```python
def callback_function(response):
  response.code # The HTTP status code
  response.headers # The HTTP headers
  response.body # The parsed response
  response.raw_body # The unparsed response
  
thread = unirest.post("http://httpbin.org/post", headers={ "Accept": "application/json" }, params={ "parameter": 23, "foo": "bar" }, callback=callback_function)

## File Uploads
Transferring file data requires that you `open` the file in a readable `r` mode:

```python
response = unirest.post("http://httpbin.org/post", headers={"Accept": "application/json"},
  params={
    "parameter": "value",
    "file": open("/tmp/file", mode="r")
  }
)
```

## Custom Entity Body

```python
import json

response = unirest.post("http://httpbin.org/post", headers={ "Accept": "application/json" },
  params=json.dumps({
    "parameter": "value",
    "foo": "bar"
  })
)
```

**Note**: For the sake of semplicity, even with custom entities in the body, the keyword argument is still `params` (instead of `data` for example). I'm looking for feedback on this.

### Basic Authentication

Authenticating the request with basic authentication can be done by providing an `auth` array like:

```python
response = unirest.get("http://httpbin.org/get", auth=('username', 'password'))
```
    
# Request

```python
unirest.get(url, headers = {}, params = {}, auth = (), callback = None)
unirest.post(url, headers = {}, params = {}, auth = (), callback = None)
unirest.put(url, headers = {}, params = {}, auth = (), callback = None)
unirest.patch(url, headers = {}, params = {}, auth = (), callback = None)    
unirest.delete(url, headers = {}, params = {}, auth = (), callback = None)
```

- `url` - Endpoint, address, or uri to be acted upon and requested information from.
- `headers` - Request Headers as associative array or object
- `body` - Request Body associative array or object
- `callback` - Asychronous callback method to be invoked upon result.

# Response
Upon receiving a response, Unirest returns the result in the form of an Object. This object should always have the same keys for each language regarding to the response details.

- `code` - HTTP Response Status Code (Example 200)
- `headers`- HTTP Response Headers
- `body`- Parsed response body where applicable, for example JSON responses are parsed to Objects / Associative Arrays.
- `raw_body`- Un-parsed response body