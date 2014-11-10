[block:api-header]
{
  "type": "basic",
  "title": "gredis - a redis extension for glim"
}
[/block]
gredis is a glim framework extension for bringing up redis features to glim framework. It uses [redis-py](https://github.com/andymccurdy/redis-py) which is the most popular redis library for python.

# Installation
- Clone the repo
- Move gredis folder into `ext`
- Remove `.git` folder
[block:code]
{
  "codes": [
    {
      "code": "# clone the repo\ngit clone git@github.com:aacanakin/glim-redis.git\n\n# move gredis folder into ext\nmv glim-redis/gredis /path/to/project/ext/.\n\n# remove .git folder inside gredis folder\nrm -rf gredis/.git",
      "language": "shell"
    }
  ]
}
[/block]
# Confguration
- Append gredis dependancy to extensions in your config file;
- Add the following config to the extensions config;
[block:code]
{
  "codes": [
    {
      "code": "config = {\n    'extensions' : {\n        'gredis' : {\n            'default' : {\n                'host' : 'localhost',\n                'port' : '6379',\n                'db'   : 0\n            }\n            # add a new dict for connecting multiple redis\n            # servers\n        }\n    },\n    # ...\n}",
      "language": "python"
    }
  ]
}
[/block]
Here `default` is used for connection aliasing. Gredis can handle multiple connections.

Start your web server and that's it!

# Usage
[block:code]
{
  "codes": [
    {
      "code": "from ext.gredis.gredis import Redis\n\n# performs operations in default connection\nRedis.set('foo', 'bar')\nRedis.set('foo', 'bar')\n\n# performs operations in aliased connection\nRedis.connection('connection-name').set('foo', 'bar')",
      "language": "python"
    }
  ]
}
[/block]