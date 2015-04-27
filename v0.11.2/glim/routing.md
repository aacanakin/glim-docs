Glim has an easy to use powerful routing feature where developers can define routes in seconds. It uses werkzeug's great routing features and exposes all of them. The route definitions reside in `app/routes.py` file. The simplest route example would be the following;
[block:code]
{
  "codes": [
    {
      "code": "# routes.py\nurls = {\n    '/': 'BaseController.hello'\n}",
      "language": "python"
    }
  ]
}
[/block]
The first route definition simply means that `BaseController`'s `hello()` function is mapped to the root route ( / ). Although most of the popular web frameworks allow automatic detection of controller, action, glim doesn't allow for that because of security reasons.
[block:api-header]
{
  "type": "basic",
  "title": "RESTful Routing"
}
[/block]
There are two ways of RESTful routing in glim. The first one is to define Controllers as resources.
Consider the following route definition;
[block:code]
{
  "codes": [
    {
      "code": "# routes.py\nurls = {\n    '/users': 'UserController'\n}",
      "language": "text"
    }
  ]
}
[/block]

The second route definition is a typical example of a `RESTful` routing in glim. This means that the following http methods are automatically mapped to the functions;
[block:parameters]
{
  "data": {
    "0-0": "/users",
    "h-0": "URL",
    "h-1": "HTTP Method",
    "h-2": "Function",
    "0-1": "POST",
    "0-2": "UserController.post()",
    "1-0": "/users",
    "1-1": "PUT",
    "1-2": "UserController.put()",
    "2-0": "/users",
    "2-1": "GET",
    "2-2": "UserController.get()",
    "3-0": "/users",
    "3-1": "DELETE",
    "3-2": "UserController.delete()",
    "4-0": "/users",
    "4-1": "PATCH",
    "4-2": "UserController.patch()"
  },
  "cols": 3,
  "rows": 5
}
[/block]
Moreover, glim has an option to define routes with single verbs. For instance;
[block:code]
{
  "codes": [
    {
      "code": "# routes.py\nurls = {\n\t'POST /authentication': 'UserController.login'\n}",
      "language": "python"
    }
  ]
}
[/block]
This route will match /authentication endpoint with only POST method to UserController's login function
[block:api-header]
{
  "type": "basic",
  "title": "More examples on routing"
}
[/block]
Glim has features to validate variables inside the route level as werkzeug provides. A more complex route definition example would be the following;
[block:code]
{
  "codes": [
    {
      "code": "# routes.py\nurls = {\n    '/feeds/<slug>' : 'FeedController.get'\n}",
      "language": "python"
    }
  ]
}
[/block]
In this example, this route definition will be mapped into `FeedController`‘s `get(slug)` function.
[block:callout]
{
  "type": "info",
  "body": "A GET | POST to /feeds route without <slug> variable will result in HTTP 404 - Not found response."
}
[/block]
You can also define types of variables in route level. The following example will also match types;
[block:code]
{
  "codes": [
    {
      "code": "# routes.py\nurls = {\n    '/post/<int:year>' : 'PostController.get'\n}\n\n# is mapping to\n\n# app/controllers.py\nclass PostController(object):\n\t\tdef get(self, year):\n      # do stuff",
      "language": "python"
    }
  ]
}
[/block]
 In this example, the route will only be mapped for integer typed year variables. The route will be mapped into `PostController`‘s `get()` function with year only being an integer.

The routing system also allows defining the length of variables. For example;
[block:code]
{
  "codes": [
    {
      "code": "# routes.py\nurls = {\n    '/index/<string(length=2):lang_code>' : 'HomeController.index'\n}",
      "language": "python"
    }
  ]
}
[/block]
In this definition, the route will be mapped `HomeController`‘s `index(lang_code)` function with `lang_code` being only strings with length of 2.

For mapping any of the routes, `<any>` keyword can be used in defining generic routes like the following;
[block:code]
{
  "codes": [
    {
      "code": "# routes.py\nurls = {\n    '/<any(about, help, imprint):page' : 'PageController.resolve'\n}",
      "language": "python"
    }
  ]
}
[/block]
In this definition, the route will be mapped into PageController‘s resolve(page) function with page variable being `about`, `help` or `imprint`.