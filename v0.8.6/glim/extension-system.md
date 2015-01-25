Glim provides an extension system where developers can further improve the functionality of glim. The core package is currently providing solutions for the common problems of web development. However, you can easily develop extensions and integrate tightly to glim framework. Ok, ready ? let's get started!
[block:api-header]
{
  "type": "basic",
  "title": "Configuration"
}
[/block]
In glim, the default configuration comes with a dict namely "extensions". In this key, the enabled app extensions and its configuration should be defined. Consider an example of a redis extension namely "gredis". The extension configuration would be the following;
[block:code]
{
  "codes": [
    {
      "code": "# app/config/<env>.py\nconfig = {\n\n  \t# ...\n  \n    'extensions': {\n    \t\t'gredis' : {\n      \t\t  'default': {\n                'host': 'localhost',\n                'port': '6379',\n                'db': 0\n            }\n        }\n    },\n  \n  \t# ...\n}",
      "language": "python"
    }
  ]
}
[/block]

This configuration simply says to glim that an extension named "gredis" should be enabled with the configuration inside the '"default" key when the web server starts. The default key exists for connection aliasing. It's there for redis extension to handle multiple redis connections.

After this configuration is enabled, glim will look for an extension (a folder) called "gredis" in `ext` folder.
[block:callout]
{
  "type": "info",
  "title": "Configuration is optional but recommended",
  "body": "In glim, the extensions can also be loaded during runtime with explicitly instantiating the classes of the extension, But the configuration above would be much more easy to boot an extension. You can use manually if an extension doesn't require to be booted up once and used everywhere."
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "The extension module and start"
}
[/block]
The following file structure could be used to create extensions;
```
gredis
├── __init__.py
├── gredis.py
├── requirements
└── start.py
```

The gredis.py holds the core classes to use them and a `Facade` class that can boot these core objects with configuration and hold the instance of it. The start.py is used bunch of statements when the extension is enabled. "requirements" can be used for 3rd party dependencies of pypi. So, let's have a look at this extension;
[block:code]
{
  "codes": [
    {
      "code": "# gredis.py\nfrom glim.core import Facade\nfrom glim.component import Extension\nfrom glim.facades import Log\n\nimport redis\n\nclass GredisExtension(Extension):\n\n\t\tdef __init__(self, config):\n        self.config = config\n        self.active = 'default'\n        self.connections = {}\n\n        for k, config in self.config.items():\n            self.connections[k] = self.connect(config)\n\n\t\tdef __getattr__(self, attr):\n        try:\n\t\t\t\t\t\treturn getattr(self.connections[self.active], attr)\n        except redis.RedisError, e:\n          \tLog.error(e)\n          \treturn None\n\n\t\tdef connection(self, key = None):\n\t\t\t\tif key:\n\t\t\t\t\tself.active = key\n\t\t\t\telse:\n\t\t\t\t\tself.active = 'default'\n\t\t\t\treturn self\n\n    def connect(self, config):\n      \ttry:\n       \t    connection = redis.StrictRedis(\n                host = config['host'],\n          \t\t\tport = config['port'],\n          \t\t\tdb = config['db'])\n\n\t        \tconnection.ping()\n        \t\treturn connection\n\n      \texcept redis.RedisError, e:\n        \t\tLog.error(e)\n        \t\treturn None\n\nclass Redis(Facade):\n\t\taccessor = GredisExtension",
      "language": "python"
    }
  ]
}
[/block]
As it can be noticed, this extension has the simplest two class to make redis available for glim. The `GredisExtension` includes of the whole implementation of the redis client. The `Redis` class is there for to persist the `GredisExtension` to the runtime. The `Redis` class is only there to keep the GredisExtension instance.

The start.py file would be the following in this case;
[block:code]
{
  "codes": [
    {
      "code": "from ext.gredis import Redis\n\ndef before(config):\n\t\tRedis.register(config)",
      "language": "python"
    }
  ]
}
[/block]
The `before` function is automatically called by glim after enabling the extension from configuration. The config is also passed by glim's extension loader. `Redis.register` statement simply makes the redis instance persist like in a singleton manner but not completely. Glim approach doesn't use any kind of private like constructors or singletons. Instead, it creates Facades to keep the instances in a static manner.
[block:callout]
{
  "type": "success",
  "title": "Best practice",
  "body": "Don't use singletons in glim. Use facades to contain object's instance instead. [Singletons are bad](http://stackoverflow.com/questions/137975/what-is-so-bad-about-singletons) and they are much more difficult to test."
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "The installation of an extension"
}
[/block]
The installation of an extension is to move the extension folder inside `ext` folder and install its dependencies. In this case the installation would be the following;
[block:code]
{
  "codes": [
    {
      "code": "$ . venv/bin/activate\n$ cp <gredis-file-path> ext/.\n$ pip install requirements",
      "language": "shell"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Manual installation.. pff.",
  "body": "Currently, the installation of a glim extension is manual. The further releases of glim will provide great feature to install and enable extensions from command line."
}
[/block]
You can check the code of other extensions;
- [glim-migrations](https://github.com/aacanakin/glim-migrations) - RDB Migrations for glim
- [glim-jobqueue](https://github.com/aacanakin/glim-jobqueue) - A redis job producing / consuming extension
- [glim-memcached](https://github.com/aacanakin/glim-memcached) - The memcached extension for glim.