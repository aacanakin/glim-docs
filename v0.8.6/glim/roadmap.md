This section includes the future features and changes of glim project.
[block:api-header]
{
  "type": "basic",
  "title": "Framework"
}
[/block]
# 0.9.x releases

- travis-ci integration
- nose integration
- logging
    + provide app, root and framework logger
    + configuration for colored logging
    + add `write()` function
- remove facades module, seperate all modules into their type
- validation module
- views
    + data sharing among templates
- exceptions
    + GlimError and its extensions
- model layer
    + renewing db connections & orm connections after db fails
    + configuration for `autocommit`
    + configuration for database echoing using `echo=True`

# 0.10.x releases
- code generator integration
    + jinja2 app component templates (py templates)
        - dir structure
        - controller, model, view, service, command, extension
    + command line tools for generating new app components
        - new controller, model, view, service, command, extension
- dependency injection
    +  google's pinject library integration
- routing, aspect orientation
    + improvements on routing
        - aliased & reusable routes
        - filters in controller level
- reusable command line apps
- add feature for services to be registered during runtime

# 0.11.x releases
- core extensions implementation
    + app object passed extensions
- event system (maybe an extension instead)
    + concurrent, sync, async events
    + actor based model
- python 3 compatibility
- extensions
    + enabling extensions from command line
        - publish configurations of extensions
    + installing extensions from command line
        - from github or bitbucket (maybe)
        - from pypi

# 0.12.x releases
- final structure for app components
    + refactor in config
    + refactor in ext folder

# 1.x releases
- bug fixes about the previous features
- who knows!
[block:api-header]
{
  "type": "basic",
  "title": "Extensions"
}
[/block]
- Mail
- Message Queue (AWSQ, Rabbit, Iron, ZeroMQ etc)
- MongoDB
- Cassandra