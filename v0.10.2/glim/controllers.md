Controllers are the heart of glim framework. They are responsible for request handling, input validation and return appropriate responses to the requests. When a new glim app is created using `$ glim new`, `app/controllers.py` will be automatically generated to be used. In glim, all the controllers reside in `controllers.py`. The simplest controller definition would be the following;
[block:code]
{
  "codes": [
    {
      "code": "# controllers.py\nfrom glim import Controller\nfrom glim import Response\n\nclass BaseController(Controller):\n    def hello():\n        return Response('Hello World Mate!')\n      \t# OR\n        # return 'Hello World Mate!'",
      "language": "python"
    }
  ]
}
[/block]
In this particular example, `hello` function can be called as an "action" from an mvc perspective. Notice that string based responses are also automatically handled by glim. The `hello` function can be mapped to a url in `routes.py`.
[block:code]
{
  "codes": [
    {
      "code": "#routes.py\nurls = {\n    '/': 'BaseController.hello'\n}",
      "language": "python"
    }
  ]
}
[/block]
In this example, when you navigate to [http://127.0.0.1:8080](http://127.0.0.1:8080) in your browser, you will notice the Response string being returned.

Controllers are automatically instantiated by glim dispatcher. Dispatcher automatically generates an instance injected to request object so that you don't need to define any functions in the static scope. There exists further information about requests and response in   the [next](doc:request--response) section.