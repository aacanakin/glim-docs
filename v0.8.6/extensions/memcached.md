[block:api-header]
{
  "type": "basic",
  "title": "glim-memcached"
}
[/block]
glim-memcached is a glim framework extension for bringing up memcached features to glim. It uses [pinterest's memcache module](https://github.com/pinterest/pymemcache).

# Installation
- clone the repo inside ext folder
- install [pymemcache](https://github.com/pinterest/pymemcache)
- move memcached folder into ext
- remove `.git` directory if exists
[block:code]
{
  "codes": [
    {
      "code": "# clone the repo\ngit clone git@github.com:aacanakin/glim-memcached.git\n\n# move the memcached folder into ext\nmv glim-memcached/memcached /path/to/project/ext/.\n\n# remove .git folder\nrm -rf memcached/.git",
      "language": "shell"
    }
  ]
}
[/block]
# Configuration
Append the following config to the extensions config;
[block:code]
{
  "codes": [
    {
      "code": "config = {\n\n    'extensions' : {\n        'memcached' : {\n            'default' : {\n                'host' : 'localhost',\n                'port' : 11211,\n            }\n            # add a new dict for connecting multiple redis\n            # servers\n        }\n    },\n    # ...\n}",
      "language": "python"
    }
  ]
}
[/block]
Here default is used for connection aliasing. Memcached can handle multiple connections.

Start your web server and that's it!

# Usage
[block:code]
{
  "codes": [
    {
      "code": "from ext.memcached.memcached import Cache\n\n# performs operations in default connection\nCache.set('foo', 'bar')\nCache.get('foo')\n\n# performs operations in aliased connection\nCache.connection('connection-name').set('foo', 'bar')",
      "language": "python"
    }
  ]
}
[/block]