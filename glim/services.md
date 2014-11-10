[block:callout]
{
  "type": "warning",
  "title": "Do not get confused!",
  "body": "Services might be different than the very first thing that comes to mind. People might get confused about it. It is just a concept that is a little bit different in glim. It's not about web service building."
}
[/block]
In glim framework, services can be used for seperating models from logic. Moreover, you can hold class wrappings in services such as mail sending, message queueing, etc. They can be defined as the reusable components that controllers can reuse. In a typical glim app, services reside in `app/services.py` file.
[block:callout]
{
  "type": "info",
  "title": "Completely Optional",
  "body": "This layer is completely optional. You may want to move database logic through controllers or models. However, services are a good place to hold db logic and some wrappings of libraries."
}
[/block]
The database logic can be seperated into number of functions in services. Consider an example of a user service. You can define a class namely "UserService" resides in `app/services.py`;
[block:code]
{
  "codes": [
    {
      "code": "# services.py\nfrom glim.db import Orm\nfrom app.models import User\n\nclass UserService:\n    @staticmethod\n  \tdef auth(email, password):\n        user = Orm.query(User.id, User.email)\n    \t\t\t\t\t\t\t.filter_by(email=email, password=password)\n    \t\treturn user",
      "language": "python"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "A Note on static functions",
  "body": "Since the services are mostly stateless. Therefore, it is clever to make the service functions as static. However, if you are hesitant about using static, you can use `classmethod` or just functions with no classes. The following example shows of a service just by defining functions;"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "# services.py\nfrom glim.db import Orm\n\ndef auth_user(email, password):\n    user = Orm.query(User.id, User.email)\n    \t\t\t\t  .filter_by(email=email, password=password)\n  \treturn user",
      "language": "python"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Where to use services and where not to ?"
}
[/block]
Since it's completely optional to make use of services, there are some guidelines about where to use services and where not to. 

# Where to use services ?
- Reusable functions for controllers

    Glim approach does not recommend reusability in the controller layer, meaning that if you                                   
    are calling reusable functions inside controllers, these functions should be on the services 
    layer. Glim recommends to move the logic to services. If you have different routes doing
     different jobs, these controllers should be mapped into seperate functions.
    
    Consider a web project having secured and non-secured requests. Suppose that clients
    can only see blog posts if they are authenticated. Moreover, they can only create blog 
    posts if they are authenticated. A bad example of implementing this would be the 
    following;
[block:code]
{
  "codes": [
    {
      "code": "# routes.py\nurls = {\n    '/login': 'AuthController.login',\n    '/post/create': 'PostController.create',\n    '/post/:<int:id>': 'PostController.get'\n}\n\n# controllers.py\nfrom glim.component import Controller\nfrom glim.db import Orm\n\nclass AuthController(Controller):\n    def login(self):\n        # perform login using Orm usage\n    def check_auth(self):\n        # perform auth checking using Orm usage\n\nclass PostController(Controller):\n    def create(self):\n        auth_controller = AuthController(self.request)\n        if auth_controller.check_auth():\n            # perform post creating\n        else:\n            # send unauthorized response\n\n    def get(self, id):\n        auth_controller = AuthController(self.request)\n        if auth_controller.check_auth():\n            # perform post getting\n        else: \n            # send unauthorized response",
      "language": "python"
    }
  ]
}
[/block]
You might notice of a little bit dirty code here. It is, actually not clean. Instead of instantiating `AuthController`, you can move this to the service layer;
[block:code]
{
  "codes": [
    {
      "code": "# routes.py\nurls = {\n    '/login': 'AuthController.login',\n    '/post/create': 'PostController.create',\n    '/post/<int:id>': 'PostController.get'\n}\n\n# services.py\nfrom glim.component import Service\n\nclass AuthService(Service):\n    @staticmethod\n    def check(self):\n        # if user is auth, then return True\n        # else, return False\n\n# controllers.py\nfrom glim.component import Controller\nfrom glim.db import Orm\nfrom app.services import AuthService\n\nclass AuthController(Controller):\n    def login(self):\n        # perform login using Orm usage\n\nclass PostController(Controller):\n    def create(self):\n        if AuthService.check():\n            # perform post creating\n        else:\n            # send unauthorized response\n\n    def get(self, id):\n        if AuthService.check():\n            # perform post getting\n        else: \n            # send unauthorized response",
      "language": "python"
    }
  ]
}
[/block]
Controllers should only dealing with requests and responses. It will be a little bit cleaner this way. This can be thought as angular.js's services.
[block:callout]
{
  "type": "info",
  "title": "The best way of auth checking would be in filters",
  "body": "The above example is also not a good practice. Use filters for auth checking instead!"
}
[/block]
- Wrapping a class w/out any db operation

   In glim, services are a good place to wrap classes and could be made static for easy to be
   called from outside. If you consider an example of a mail sending logic, you would put this 
   logic inside services

  Consider an example of a system having approval mails after a successful registration. 
  This app would be the following;
[block:code]
{
  "codes": [
    {
      "code": "# routes.py\nurls = {\n    '/user/register': 'UserController.register',\n    '/user/approve': 'UserController.approve'\n}\n\n# services.py\nfrom glim.component import Service\nclass MailService(Service):\n    @staticmethod\n    def send_approval_mail(from, to, body):\n        # perform mail sending\n        # return if mail is sent\n\n# controllers.py\nfrom glim.component import Controller\nfrom app.services import MailService\n\nclass UserController(Controller):\n    def register(self):\n        # perform registering using services or just here\n        from = 'blah'\n        to = 'blah'\n        body = 'Welcome to glim my friend!'\n        result = MailService.send_approval_mail(from, to, body)\n        # return appropriate response\n    def approve(self):\n        # perform approval using services or just here\n        # return appropriate response",
      "language": "python"
    }
  ]
}
[/block]
In this example, the mail service is used as a wrapper for a mail library. Therefore, as you might notice, services can be pretty useful when it comes to library wrapping.

# Where not to use services ?
- For simplicity, if you have not much reusable database logic.
  Services can be pretty complex and useless when every single controller has its own 
  service. Therefore, you may want to move the database logic if it's not reusable in other 
  controllers. This can be done on small apps or even bigger apps. Services are for keeping 
  reusability in controller layer without creating functions inside controllers that are not 
  mapped to routes.

  However, if you need library mapping, services would still be the greatest place to hold that.
[block:callout]
{
  "type": "info",
  "title": "Again not a must, but recommended",
  "body": "These are just recommended practices glim framework is offering, you can make it different as much as you want."
}
[/block]