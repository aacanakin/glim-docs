In glim framework, requests and responses are handled by application controllers. There exists a `Controller` class in `glim.component` module that every controllers must extend. The request & response objects are just wrappers of Werkzeug's `BaseRequest` & `BaseResponse`.
[block:api-header]
{
  "type": "basic",
  "title": "Request"
}
[/block]
After a request is mapped into a controller, the request variables are automatically injected into Controller. Since all the application controllers must extend `Controller` class, it is not needed to set the request explicitly for each controller. The base controller class is the following;
[block:code]
{
  "codes": [
    {
      "code": "class Controller:\n    \"\"\"\n\n    The controller component is responsible for handling requests\n    and returning appropriate response to the requests.\n\n    Attributes\n    ----------\n      request (werkzeug.wrappers.Request): The current request\n        object. This object is automatically passed by dispatch\n        module.\n\n    \"\"\"\n    def __init__(self, request):\n        self.request = request",
      "language": "python"
    }
  ]
}
[/block]
Simple, isn’t it? So, each controller can access the request object by `self.request` that is provided by werkzeug dispatcher. `self.request` is an alias of `werkzeug.wrappers.BaseRequest` object that is injected by dispatcher.

Some useful functions for Request are the following;
[block:code]
{
  "codes": [
    {
      "code": "self.request.environ  # the wrapped wsgi environ dict\nself.request.app      # bottle application object\nself.request.route    # current Route object\nself.request.url_args # arguments that are extracted from url\nself.request.method   # request method of current route\nself.request.headers  # request headers as dict\nself.request.get_header(name, default=None) # retrieve header\nself.request.cookies  # cookies parsed into FormsDict\nself.request.get_cookie(key, default=None, secret=None) # retrieve cookie\nself.request.query.<name>  # retrieve query string key\nself.request.forms.<name>  # retrieve form variable\nself.request.params.<name> # retrieve form & query combined vars\nself.request.json # retrieve json body\nself.request.body # retrieve raw body\nself.request.is_xhr # flag of request triggered by XMLHttpRequest\nself.request.is_ajax # flag of ajax\nself.request.auth # http authentication data\nself.request.remote_addr # remote ip address\nself.request.remote_route # list of all ips with X-Forwarded-For header\nself.request.copy() # make a shallow copy of request",
      "language": "python"
    }
  ]
}
[/block]
A much more extensive documentation about Request objects can be found [here](http://bottlepy.org/docs/dev/api.html#bottle.BaseRequest)
[block:api-header]
{
  "type": "basic",
  "title": "Response"
}
[/block]
The responses are the returned variables of any application `Controller` class in glim. For example;
[block:code]
{
  "codes": [
    {
      "code": "# controllers.py\nfrom app.models import Post\nfrom glim import Controller\nfrom glim import Response\nfrom glim_extensions.db import Orm\n\nimport json\n\nclass PostController(Controller):\n    def get(id):\n        response = {}\n        response['error'] = None\n        return response\n        # returns {\"error\": null}",
      "language": "python"
    }
  ]
}
[/block]
This is a simple REST like web service example of a `PostController`. Notice that, Response is a type of `werkzeug.wrappers.BaseResponse`. You can also drop the use of `return Response()` statement and use only `return json.dumps(response)`. Glim will understand this and create a Response for you.
[block:callout]
{
  "type": "warning",
  "title": "Do not use string based returns in filters",
  "body": "Although glim can convert the string returns into Responses, it is highly suggested not to use string base returns in filters for not having ambiguous definitions."
}
[/block]
Some useful variables for generating response;
[block:parameters]
{
  "data": {
    "0-0": "headers",
    "h-0": "variable",
    "h-1": "description",
    "0-1": "a list of headers or a Headers object",
    "1-0": "status_code",
    "1-1": "the http status code of response",
    "2-0": "mimetype",
    "2-1": "the mimetype for the request",
    "3-0": "content_type",
    "3-1": "the content type for the request",
    "4-0": "get_header(name, default=None)",
    "5-0": "set_header(name, value)",
    "6-0": "add_header(name, value)",
    "4-1": "retrieve header key",
    "5-1": "set header key - value",
    "6-1": "add header key - value",
    "7-0": "content_type",
    "7-1": "shortcut of Content-Type"
  },
  "cols": 2,
  "rows": 8
}
[/block]
A much more extensive documentation of Response objects can be found [here](http://bottlepy.org/docs/dev/api.html#bottle.BaseResponse)