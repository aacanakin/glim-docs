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
Consider the following route definition;
[block:code]
{
  "codes": [
    {
      "code": "# routes.py\nurls = {\n    '/blog/post': 'BlogPostController'\n}",
      "language": "text"
    }
  ]
}
[/block]

The second route definition is a typical example of a `RESTful` routing in glim. This means that the following http methods are automatically mapped to the functions;
[block:parameters]
{
  "data": {
    "0-0": "/blog/post",
    "h-0": "URL",
    "h-1": "HTTP Method",
    "h-2": "Function",
    "0-1": "POST",
    "0-2": "BlogPostController.post()",
    "1-0": "/blog/post",
    "1-1": "PUT",
    "1-2": "BlogPostController.put()",
    "2-0": "/blog/post",
    "2-1": "GET",
    "2-2": "BlogPostController.get()",
    "3-0": "/blog/post",
    "3-1": "DELETE",
    "3-2": "BlogPostController.delete()",
    "4-0": "/blog/post",
    "4-1": "PATCH",
    "4-2": "BlogPostController.patch()"
  },
  "cols": 3,
  "rows": 5
}
[/block]

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
      "code": "# routes.py\nurls = {\n    '/post/<int:year>' : 'PostController.get'\n}",
      "language": "python"
    }
  ]
}
[/block]
 In this example, the route will only be mapped for integer typed year variables. The route will be mapped into `PostController`‘s `get(year)` function with year only being an integer.

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
[block:api-header]
{
  "type": "basic",
  "title": "Filtering"
}
[/block]
What makes the routing system powerful is mostly because glim allows filtered & grouped route definitions. "Filtering" means here **to run other controller functions before the defined controller runs**. This features make really use of controllers having states without any `if` `else` statements. The following example shows a simple filter definition;
[block:code]
{
  "codes": [
    {
      "code": "# routes.py\nurls = {\n    '/' : [\n        'BaseController.validate',\n        'BaseController.hello'\n    ]\n}",
      "language": "python",
      "name": null
    }
  ]
}
[/block]
Here, this route definition simply means the following statement;

"**Run the `validate` function before the `hello` function. If validate returns `Response`, then don't run the `hello` function**".
[block:callout]
{
  "type": "warning",
  "title": "Be careful in defining filters",
  "body": "The routing system won't run the defined controller if your filter function returns an object type of `Response`. In the example above, validate should not return anything if the request is a valid request."
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "No alias of filters",
  "body": "In filter definition, each request filter should be explicitly defined inside the route definition. Currently, there is no support of aliasing routes for simplicity. This issue will be addressed in further releases of glim"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Grouping"
}
[/block]
Consider an example of a web app having authentication system. Filtering can be used here by the following;
[block:code]
{
  "codes": [
    {
      "code": "# routes.py\nurls = {\n    '/api' : {\n        '/auth' : 'ApiController.auth',\n        '/me' : [\n            'ApiController.check_auth',\n            'ApiController.me'\n        ]\n    }\n}",
      "language": "python"
    }
  ]
}
[/block]
In this example, the following routes will be registered;
[block:parameters]
{
  "data": {
    "h-0": "url",
    "0-0": "/api/auth",
    "h-1": "mapping",
    "0-1": "ApiController.auth",
    "1-0": "/api/me",
    "h-2": "filter",
    "0-2": "-",
    "1-1": "ApiController.me",
    "1-2": "ApiController.check_auth"
  },
  "cols": 3,
  "rows": 2
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Repeated grouping",
  "body": "In route grouping, you need to define filters for each endpoint repeatedly for not having ambiguous definition for routes. This issue will be addressed in further releases of glim."
}
[/block]