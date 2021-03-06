[block:callout]
{
  "type": "info",
  "title": "We are living in a single page world",
  "body": "Although glim has a fully featured view layer to support server side templates, currently there exists single page fashion which allows client side templating. Therefore, although this is completely optional, it is highly suggested not to use server side view rendering for keeping the server only busy with data retrieval. Instead, use some great javascript frameworks like [angular.js](https://angularjs.org/)"
}
[/block]
# Jinja2 extension for glim framework
After 0.9 releases of glim, view layer is separated from glim core and served as an extension. Therefore, it is required to install glim-extensions module using pip
[block:code]
{
  "codes": [
    {
      "code": "pip install glim-extensions",
      "language": "shell"
    }
  ]
}
[/block]
# Configuration
[block:code]
{
  "codes": [
    {
      "code": "# app/config/<env>.py\nconfig = {\n    'extensions': {\n       'view': {\n           'package': 'app.views'\n       },\n    }\n}",
      "language": "python"
    }
  ]
}
[/block]

There exists a really simple jinja2 integration in glim. The view layer in glim simply is a set of jinja2 templates. Glim views can be defined in app/views folder. The right place to render templates would be inside controllers. So, let’s make a controller return a jinja2 template;
[block:code]
{
  "codes": [
    {
      "code": "# controllers.py\nfrom glim_extensions.view import View\nfrom glim import Controller\nfrom app.models import Post\nfrom glim_extensions.db import Orm\n\nclass PostController(Controller):\n    def get(id):\n        p = Orm.query(Post.post_id, Post.content).filter_by(post_id=id).first()\n        if p:\n            post = dict(zip(post.keys(), p))\n            return View.render('post', post=post)\n        else:\n            return Response(status=404)",
      "language": "python"
    }
  ]
}
[/block]
The template file would be the following;
[block:code]
{
  "codes": [
    {
      "code": "# views/post.html\n<html>\n    <body>\n    Post id : {{ post.id }}\n    Post content : {{ post.content }}\n    </body>\n</html>",
      "language": "html"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Logic in templates"
}
[/block]
## Looping
## If - Else Statements

[block:api-header]
{
  "type": "basic",
  "title": "Including templates"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Extending templates"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Sharing data between templates"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Configuring template path"
}
[/block]