After we have a working glim app, now it can be further learned what's inside a typical glim app and some configuration tips.
[block:api-header]
{
  "type": "basic",
  "title": "App Structure"
}
[/block]
`$ glim new` command creates the following directory structure;
```
app
├── commands.py        => custom command line utilities
├── config             => configuration sources
│   └── default.py     => a sample default config
|   └── development.py => the development environment config
├── controllers.py     => controller sources
├── models.py          => model layer sources
├── routes.py          => application routes source
├── services.py        => application services source
├── static             => static folder to hold css,img,js,etc.
├── start.py           => functions run before the web app starts
└── storage            => storage folder for logs, sessions, etc.
ext                    => ext folder to keep extensions
```
[block:api-header]
{
  "type": "basic",
  "title": "Configuration"
}
[/block]
Glim provides a structure to keep application level constants. `app/config` folder has application level configuration. After creating a sample app, glim automatically creates this folder.

By default, `app/config` has two files namely `default.py` & `development.py`. The name of these files are mapped to the possible environments to run the app. The default environment is `development`, which is mapped into `development.py`. In this folder, it's highly suggested not to use `default.py`. This file is there because of backup purposes.

The simplest configuration file would be the following;
[block:code]
{
  "codes": [
    {
      "code": "# config.py\nimport os\nimport glim.paths\n\nconfig = {\n\n    'extensions' : {\n        # 'gredis' : {\n        #   'default' : {\n        #       'host' : 'localhost',\n        #       'port' : '6379',\n        #       'db'   : 0\n        #   }\n        # }\n    },\n\n    # database configuration\n    'db' : {\n        # 'default' : {\n        #   'driver' : 'mysql',\n        #   'host' : 'localhost',\n        #   'schema' : 'test',\n        #   'user' : 'root',\n        #   'password' : '',\n        # },\n    },\n\n    'orm' : False,\n\n    'log' : {\n        # 'level' : 'info',\n        # 'format' : '[%(levelname)s] : %(message)s',\n        # 'file' : 'app/storage/logs/debug.log'\n    },\n\n    'views' : {\n        # package to be loaded by jinja2\n        'package' : 'app.views'\n    },\n\n    'sessions' : {\n        # session id prefix\n        'id_header' : 'glim_session',\n        'path' : glim.paths.STORAGE_PATH,\n    },\n\n    'app' : {\n        'reloader' : True,\n        'debugger' : True,\n        'static' : {\n            'path' : glim.paths.STATIC_PATH,\n            'url'  : '/static'\n        }\n    }\n}",
      "language": "python"
    }
  ]
}
[/block]
As you might notice, glim can be run without any db connection. Most of these configuration variables have internal default values. So, even if they don't exist, the default variables will be assigned automatically by glim. However, currently, it is required to have at least the keys in the dictionary. Some of the keys and their usages are the following;
[block:callout]
{
  "type": "warning",
  "title": "Don't remove the keys, instead, remove the contents of the keys",
  "body": "For not re remembering all those keys, it is highly suggested to change & remove the values of the keys but not removing the keys itself. Moreover, glim may require the existence of these keys even it doesn't require the values."
}
[/block]

[block:parameters]
{
  "data": {
    "0-0": "extensions",
    "h-0": "Key",
    "h-1": "Usage",
    "0-1": "Dynamic extension loading and keeping the extension configurations with respect to different environments",
    "1-0": "db, orm",
    "1-1": "Used for db connection configuration. Note that orm enabling is different from db configuration. Once db is configured, orm can be enabled or disabled using this flag.",
    "2-0": "log",
    "2-1": "Holds configuration for logging level of the environment, the logging format and the file name if provided. There exist a default logging setting if you don't provide one.",
    "3-0": "views",
    "3-1": "Holds configuration about the view layer. Currently, the only configurable thing is to change  views module name into a other name. Future releases address more extensible view layer.",
    "4-0": "sessions",
    "4-1": "Holds configuration for sessioning. The session header key and the storage path of sessions can be changed. If you don't provide a session configuration, session support will automatically be disabled by the framework.",
    "5-0": "app",
    "5-1": "This key holds configuration mosty about the web server. If you are familiar with werkzeug, you may notice these keys are used by the wsgi of glim. Note that glim serves the static files using /static url with static folder."
  },
  "cols": 2,
  "rows": 6
}
[/block]
The keys can be accessed from the application using the `glim.facades` module. There exists a `Config` instance to access, modify, flush these keys;
[block:code]
{
  "codes": [
    {
      "code": "from glim.facades import Config\n\nConfig.get('db')\n# returns db dict\n\nConfig.get('db.default.user')\n# returns 'root'\n\nConfig.set('db.default.password', 'glim-rocks')\n# sets config['db']['default']['password'] as 'glim-rocks'",
      "language": "python"
    }
  ]
}
[/block]
## Adding key/values to config
You can add other constants simply by adding a new key to the `config dict`. For instance, if you have a facebook connect in your glim app, you can append the following and have access by the following `Config` function call;
[block:code]
{
  "codes": [
    {
      "code": "# app/config/<env>.py\n# append this key inside config\n{\n    'facebook': {\n        'key': 'your-application-key',\n        'secret': 'your-application-secret'\n    }\n}\n\n# access these keys by the following;\nfrom glim.facades import Config\n\nConfig.get('facebook.key') # returns the key\nConfig.get('facebook.secret') # returns the secret\n",
      "language": "python"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Environments"
}
[/block]
As it is mentioned above, glim has support for multi environment configurations. In glim, each configuration file in `app/config` maps to an environment. By default, the `development` environment is enabled for the app. You can start the app with different environments using the following command;
[block:code]
{
  "codes": [
    {
      "code": "glim start --env production",
      "language": "shell"
    }
  ]
}
[/block]
In the example above, glim searches for `app/config/production.py` to load to the `Config` class.

## Adding new environments
Suppose that you need a testing environment. This environment can easily be generated and started by the following;
[block:code]
{
  "codes": [
    {
      "code": "cp app/config/default.py app/config/testing.py\n\nglim start --env testing",
      "language": "shell"
    }
  ]
}
[/block]