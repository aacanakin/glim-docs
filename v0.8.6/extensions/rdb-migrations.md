[block:api-header]
{
  "type": "basic",
  "title": "glim-migrations"
}
[/block]
glim-migrations is an extension of glim framework for bringing up rdb migrations. An rdb migrations can be defined as a change in the database of an application.

# Installation
- Clone the repo, move migration folder into ext folder
- Remove `.git` folder
- Enable Orm and database from configuration
[block:code]
{
  "codes": [
    {
      "code": "# clone the repo\ngit clone git@github.com:aacanakin/glim-migrations.git\n\n# move job folder to ext folder\nmv glim-migrations/migration /path/to/project/ext/.\n\n# remove .git folder inside migration folder\nrm -rf migration/.git",
      "language": "shell"
    }
  ]
}
[/block]
# Configuration
[block:code]
{
  "codes": [
    {
      "code": "config = {\n    'extensions' : {\n        'migration' : {\n            'db' : 'default' # the connection alias\n        }\n    },\n\n    'db' : {\n        'default' : {\n            'driver' : 'mysql',\n            'host' : 'localhost',\n            'schema' : 'test',\n            'user' : 'root',\n            'password' : ''\n        }\n    }\n\n    # it is required to have at least one db connection\n}",
      "language": "python"
    }
  ]
}
[/block]
# Initializing Migration Extension
[block:code]
{
  "codes": [
    {
      "code": "glim migration:init\n# this will create a glim_migrations table using rdb connection\n# also, it will create app/migrations.py file",
      "language": "shell"
    }
  ]
}
[/block]
# Syncing models into db
Suppose that you have the following model in your models;
[block:code]
{
  "codes": [
    {
      "code": "# app/models.py\nfrom glim.db import Model\nfrom sqlalchemy import Column, Integer, String\n\nclass User(Model):\n    __tablename__ = 'users'\n    id = Column(Integer, primary_key=True)\n    name = Column(String(255))\n    fullname = Column(String(255))\n    password = Column(String(255))\n",
      "language": "python"
    }
  ]
}
[/block]
You can easily sync with db using following;
[block:code]
{
  "codes": [
    {
      "code": "glim migration:sync --name User\n# OR\nglim:migration:sync # syncs all your db models",
      "language": "shell"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "This command will not sync if db table already exists.",
  "title": "Won't sync if db table exists"
}
[/block]
# Create a migration
[block:code]
{
  "codes": [
    {
      "code": "# app/migrations.py\nfrom app.models import User\nfrom ext.migration.migration import Migration\n\nclass AddUserMigration(Migration):\n\n    description = 'creates a sample user in users table'\n\n    def run(self):\n        user = User(id=5, name='Aras')\n        self.orm.add(user)\n        self.orm.commit()\n        return True\n\n    def rollback(self):\n        user = self.orm.query(User).filter_by(id=5).first()\n        if user is not None:\n            self.orm.delete(user)\n            self.orm.commit()",
      "language": "python"
    }
  ]
}
[/block]
To migrate this run the following;
[block:code]
{
  "codes": [
    {
      "code": "glim migration:run --name AddUserMigration\n# output\n# Migrating AddUserMigration..\n# Done.",
      "language": "shell"
    }
  ]
}
[/block]
Optionally, you can migrate all with the following command;
[block:code]
{
  "codes": [
    {
      "code": "glim migration:run # migrates all defined migrations",
      "language": "shell"
    }
  ]
}
[/block]
This command will add a sample user & add a row into `glim_migrations` table.

# Rollback a migration
[block:code]
{
  "codes": [
    {
      "code": "glim migration:rollback --name AddUserMigration\n# output\n# Rolling back AddUserMigration\n# Done.",
      "language": "shell"
    }
  ]
}
[/block]
Optionally, you can rollback all migrations ordered by `created_at desc`;
[block:code]
{
  "codes": [
    {
      "code": "glim migration:rollback # rollbacks all migrations",
      "language": "shell"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Do not change anything in glim_migrations",
  "body": "Please do not change anything in glim_migrations table manually if you don't want the migrations extension to crash."
}
[/block]