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
      "code": "# config.py\nimport os\nimport glim.paths\n\nimport os\nimport glim.paths\n\nconfig = {\n\n    # the configurations of extensions\n    'extensions': {\n        # 'gredis' : {\n        #   'default' : {\n        #       'host' : 'localhost',\n        #       'port' : '6379',\n        #       'db'   : 0\n        #   }\n        # }\n    },\n\n    # logging configuration, it has a default configuration\n    # if you don't provide one.\n    'log' : {\n\n        'app' : {\n          'level': 'info',\n          'format': '[%(levelname)s] - application : %(message)s',\n          'colored': True\n          # 'file' : 'app/storage/logs/app.log'\n        },\n\n        'glim' : {\n            'level' : 'info',\n            'format' : '[%(levelname)s] : %(message)s',\n            'colored': True\n            # 'file' : 'app/storage/logs/glim.log'\n        },\n\n    },\n\n    # the glim.app.App configuration\n    'app': {\n        'server': {\n            'host': '127.0.0.1',\n            'port': 8080,\n            'wsgi': 'wsgiref',\n            'reloader': True,\n            'debugger': True,\n            'options': {}\n        },\n        'assets': {\n            'path': glim.paths.ASSET_PATH,\n            'url': '/assets'\n        }\n    }\n}",
      "language": "python"
    }
  ]
}
[/block]
Most of these configuration variables have internal default values. So, even if they don't exist, the default variables will be assigned automatically by glim. However, currently, it is required to have at least the keys in the dictionary. Some of the keys and their usages are the following;
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
    "1-0": "log",
    "1-1": "Holds configuration for logging level of the environment, the logging format and the file name if provided. There exist a default logging setting if you don't provide one.",
    "2-0": "app",
    "2-1": "This key holds configuration mosty about the web server. If you are familiar with bottle , you may notice these keys are used by the wsgi of glim. Note that glim serves the static files using /assets url with assets folder."
  },
  "cols": 2,
  "rows": 3
}
[/block]
The keys can be accessed from the application using the `Config` module. There exists a `Config` instance to access, modify, flush these keys;
[block:code]
{
  "codes": [
    {
      "code": "from glim import Config\n\n# deeply get using dot \".\" notation\nConfig.get('app.server.host')\n# returns 127.0.0.1\n\nConfig.set('app.server.host', 'localhost')\n# sets app.server.host as localhost",
      "language": "python"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Use Config.set() function smartly!",
  "body": "Although configuration should be readonly, there exists the set() function for setting configuration in runtime. Don't use set() function unless there is an absolute need."
}
[/block]
## Adding key/values to config
You can add other constants simply by adding a new key to the `config dict`. For instance, if you have a facebook connect in your glim app, you can append the following and have access by the following `Config` function call;
[block:code]
{
  "codes": [
    {
      "code": "# app/config/<env>.py\n# append this key inside config\n{\n    'facebook': {\n        'key': 'your-application-key',\n        'secret': 'your-application-secret'\n    }\n}\n\n# access these keys by the following;\nfrom glim import Config\n\nConfig.get('facebook.key') # returns the key\nConfig.get('facebook.secret') # returns the secret\n",
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