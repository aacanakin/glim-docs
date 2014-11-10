[block:callout]
{
  "type": "warning",
  "title": "Not Complete",
  "body": "This documentation is not complete currently. You can check the [github page](http://github.com/aacanakin/glim) and the current [docs](http://aacanakin.github.io/glim) for more information."
}
[/block]
In one sentence, glim is a modern web framework on top of [Werkzeug](http://werkzeug.pocoo.org/), [SQLAlchemy](http://www.sqlalchemy.org/) and [Jinja2](http://jinja.pocoo.org/docs/dev/). The aim is to build a lightweight architecture for painless web app development in python.

The development philosophy here is to make the core small as possible but still not featureless. Therefore, the features that are not common in every web app are decomposed into extensions of glim. It is inspired from great web frameworks like [play](https://www.playframework.com/) and [laravel](http://laravel.com/).
[block:api-header]
{
  "type": "basic",
  "title": "Features"
}
[/block]
Currently, glim has all features to make a typical crud web app or a web service. Some of them are the following;
- A powerful [routing](doc:routing) system which has grouping & filtering
- A [model](doc:database--orm) layer with use of SQLAlchemy
- A [view](doc:views--templating) layer with use of Jinja2
- A [controller](doc:controllers) layer for request handling, service calling, etc.
- A [service](doc:services) layer to seperate database layer logic from models
- An object oriented [command line](doc:commands) layer
- An [extension system](doc:extension-system) that allow developers to integrate extensions to the framework
[block:callout]
{
  "type": "info",
  "title": "Roadmap",
  "body": "The feature set is currently evolving because glim is really new. You can check the [roadmap](doc:controllers) page for features of the future releases"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Raison d'être"
}
[/block]
The development of this framework is a result of my personal curiosity to the advanced topics of python.

Although there are great web frameworks for python out there, glim development approach is completely different. It is a little bit tough to know how much differences there are on the python world when comparing to other programming languages. If you are coming from a different programming languages, you may notice some conceptual differences between other programming languages. Glim is about bringing the common practices that achieved consensus through the web world so that a non pythonista can also feel like home.

Moreover, it is obvious that glim is not reinventing the wheel. It has dependencies to the best python open source projects out there. Glim is about providing the functionality of these interfaces that are put into a beautiful box.
[block:api-header]
{
  "type": "basic",
  "title": "A note on other frameworks"
}
[/block]

[block:callout]
{
  "type": "success",
  "title": "Flask & Django",
  "body": "I had a chance to flirt with both of these great frameworks. I think they have many advantages in terms of community. However, people keep asking me to compare the frameworks because they tend to be biased at the first look after seeing the dependency list. So, no harm intended. It's just a part that exposes the differences."
}
[/block]

At the first glance, after seeing the library dependencies of glim (Werkzeug, Jinja2, SQLAlchemy), one might ask the most fair question;

**How is it different from Flask ?**

It is obvious that the creators of flask doesn't want to direct strictly developers in terms of building an app structure, configuration structure, etc. Therefore, it is not a convention over configuration framework. However, glim is all about conventions, directing to the best practices, etc. So, there are lots of ways to implement a feature in flask. However, in glim, although the same solutions can be done very differently, it guides through the solution which is hard to make different.

Consider an example of an rdb integration. Flask has a documentation on how to integrate SQLAlchemy with a single `app.py` really easily. However, the `app.py` relatively different than a simple "Hello World" app. From glim framework's perspective, this kind of difference should not happen. In glim, there is the `db` module that exposes the features of SQLAlchemy while still keeping the framework lightweight because, it takes 4 lines of code to comment out in configuration to completely disable `db` module. So, glim may be named much more closer to the "full-stack" approach.

"So.. you say it's more full-stack, then.."

**How is it different from django ?**
- Not answered yet :)

[block:api-header]
{
  "type": "basic"
}
[/block]