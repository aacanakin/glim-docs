[block:api-header]
{
  "type": "basic",
  "title": "glim-memcached"
}
[/block]
glim-memcached is a glim framework extension for bringing up memcached features to glim. It uses [pinterest's memcache module](https://github.com/pinterest/pymemcache).

# Installation
- install [pymemcache](https://github.com/pinterest/pymemcache)
[block:code]
{
  "codes": [
    {
      "code": "# install glim-extensions from pip\npip install glim-extensions",
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
      "code": "# app/config/<env>.py\nconfig = {\n    'extensions' : {\n        'memcached' : {\n            'default' : {\n                'host' : 'localhost',\n                'port' : 11211,\n            }\n            # add a new dict for connecting multiple redis\n            # servers\n        }\n    },\n    # ...\n}",
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
      "code": "from glim_extensions.memcached import Cache\n\n# performs operations in default connection\nCache.set('foo', 'bar')\nCache.get('foo')\n\n# performs operations in aliased connection\nCache.connection('connection-name').set('foo', 'bar')",
      "language": "python"
    }
  ]
}
[/block]