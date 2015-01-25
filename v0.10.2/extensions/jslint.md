JSLint extension is used for auto javascript linting while the glim web server starts. It uses jslint npm module to run linting from command line.

# Installation
- install jslint from npm
[block:code]
{
  "codes": [
    {
      "code": "npm install -g jslint",
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
      "code": "# app/config/<env>.py\nconfig = {\n\t'extensions' : {\n\t\t'jslint': {\n            'source': os.path.join(paths.APP_PATH, 'assets/js'),\n        }\n    },\n\t# ...\n}",
      "language": "python"
    }
  ]
}
[/block]
# Roadmap
- Autolint after file changes without server start
- Show lint errors on served page