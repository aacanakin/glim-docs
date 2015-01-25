less is an extension for glim framework to compile less files during the web server start. It uses less npm module to compile less source from command line.

# Installation
- install less from npm
[block:code]
{
  "codes": [
    {
      "code": "npm install -g less",
      "language": "shell"
    }
  ]
}
[/block]
# Configuration
- Add the following config to the extensions config;
[block:code]
{
  "codes": [
    {
      "code": "# app/config/<env>.py\nconfig = {\n\t'extensions' : {\n  \t'less': {\n    \t'source': os.path.join(paths.APP_PATH, 'assets/less/app.less'),\n      'destination': os.path.join(paths.APP_PATH, 'assets/css/app.css'),\n      'options': [\n        'compress',\n        'lint',\n        'nojs',\n        'units'\n      ]\n    }\n  },\n\t# ...\n}",
      "language": "python"
    }
  ]
}
[/block]
- The default configuration is the following;
[block:code]
{
  "codes": [
    {
      "code": "DEFAULT_CONFIG = {\n    'source': os.path.join(paths.APP_PATH, 'assets/less/main.less'),\n    'destination': os.path.join(paths.APP_PATH, 'assets/css/main.css'),\n    'options': [\n        'lint',\n        'units',\n        'verbose'\n    ]\n}",
      "language": "python"
    }
  ]
}
[/block]
# Usage
- put a main.less file and add some styles
- add main.css import to your html sources
- start the web server
[block:code]
{
  "codes": [
    {
      "code": "<html>\n    <head>\n      <link rel=\"stylesheet\" type=\"text/css\" href=\"/assets/css/main.css\">\n    </head>\n</html>",
      "language": "html"
    }
  ]
}
[/block]
# Roadmap
- Autocompile after file changes without web server start
- Show less errors on werkzeug served page