[block:callout]
{
  "type": "info",
  "title": "db is an extension after 0.9 releases",
  "body": "By the help of community feedback, db is now a glim framework extension. Therefore, it is required to install glim-extensions package."
}
[/block]
Glim framework has an extension to support rdb abstraction by its [SQLAlchemy](http://docs.sqlalchemy.org/en/rel_0_9/) integration. SQLAlchemy is great a rdb abstraction kit with connection layer & object relational layer abstractions. In glim, the DB API won't require Orm features if it's not enabled. However, ORM is dependent on DB API naturally. It is highly suggested to read an introduction to sqlalchemy before going further.

So, let's make configuration!
[block:api-header]
{
  "type": "basic",
  "title": "Configuration & Connection Aliasing"
}
[/block]
After the 0.9.x releases, glim framework now supports sqlalchemy as an extension. Therefore, it is required to put db conifguration to the `extensions` key in config. An rdb configuration is quite simple in glim framework. Simply add these lines to enable db api & orm components;
[block:code]
{
  "codes": [
    {
      "code": "# app/config/development.py\n{\n    # ...\n  \t'extensions': {\n  \n      'db': {\n          'default': {\n              'driver': 'mysql',\n              'host': 'localhost',\n              'schema': 'test',\n              'user': 'root',\n              'password': '',\n        \t\t\t'orm': True\n          }\n      },\n    }\n}",
      "language": "python"
    }
  ]
}
[/block]
You might notice that we need a "default" key in this configuration. The reason why default exists is that glim can provide multiple database connections. The "default" key is here instructing glim to make database calls without having to give schema name. You can add more database connections by adding more keys inside the "db" dictionary;
[block:code]
{
  "codes": [
    {
      "code": "# app/config/development.py\n{\n    # ...\n    'extensions': {\n        'db': {\n            'default': {\n                'driver': 'mysql',\n              \t'host': 'localhost',\n              \t'schema': 'test',\n              \t'user': 'root',\n              \t'password': '',\n        \t\t\t\t'orm': True\n          \t},\n      \t\t\t'analytics': {\n        \t\t\t\t'driver': 'mysql',\n\t\t\t\t\t\t\t  'host': 'localhost',\n\t\t\t\t\t \t\t\t'schema': 'analytics',\n\t\t\t\t\t \t\t\t'user': 'root',\n\t\t\t\t\t \t\t\t'password': '',\n        \t\t\t\t'orm': False\n        \t\t}\n      \t},\n    }\n}",
      "language": "python"
    }
  ]
}
[/block]
Now, we have an aliased database connection called "analytics". An aliased connection could be reached in DB API like the following;
[block:code]
{
  "codes": [
    {
      "code": "from glim_extensions.db import Database as DB\n# returns the connection\nDB.connection('analytics') # returns the \"analytics\" connection.\nDB.connection() # returns the default connection",
      "language": "python"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Defining Models & Mapping"
}
[/block]
Glim has a Model class inside glim.db module. The Model class is simply an alias of `declarative_base()` in SQLAlchemy. Defining a simple User model would be the following;
[block:code]
{
  "codes": [
    {
      "code": "# app/models.py\nfrom glim_extensions.db import Model\nfrom sqlalchemy import Column, Integer, String\n\nclass User(Model):\n    __tablename__ = 'users'\n    id = Column(Integer, primary_key=True)\n    name = Column(String(255))\n    fullname = Column(String(255))\n    password = Column(String(255))",
      "language": "python"
    }
  ]
}
[/block]
This model maps to a users database table given the properties. As you noticed, the table name is mapped using `__tablename__` variable. In other words, this table is mapped as if there exists a table with the following sql;
[block:code]
{
  "codes": [
    {
      "code": "CREATE TABLE `users` (\n  `id` int(11) NOT NULL,\n  `name` varchar(255) DEFAULT NULL,\n  `fullname` varchar(255) DEFAULT NULL,\n  `password` varchar(255) DEFAULT NULL,\n  PRIMARY KEY (`id`)\n);",
      "language": "mysql"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Defining Relations"
}
[/block]
In glim, you can define model relations with SQLAlchemy's `relationship()` function. This section provides model relation examples.

# One to One
SQLAlchemy defines one to one relations by the following statement;

"One To One is essentially a bidirectional relationship with a scalar attribute on both sides. To achieve this, the uselist flag indicates the placement of a scalar attribute instead of a collection on the “many” side of the relationship."

Consider a system where users having a phone number. In glim, you can define 2 tables namely "User" and "Phone" and provide a one to one relationship with the following;
[block:code]
{
  "codes": [
    {
      "code": "# models.py\nfrom glim_extentions.db import Model\nfrom sqlalchemy import Column, Integer, String, Text, ForeignKey\nfrom sqlalchemy.orm import relationship, backref\n\nclass User(Model):\n    __tablename__ = 'users'\n    id = Column(Integer, primary_key=True)\n    name = Column(String(255))\n    fullname = Column(String(255))\n    password = Column(String(255))\n    phone = relationship(\"Phone\", uselist=False, backref=\"phones\")\n\nclass Phone(Model):\n\t\t__tablename__ = 'phones'\n\t\tid = Column(Integer, primary_key=True)\n\t\tuser_id = Column(Integer, ForeignKey('users.id'))\n\t\tnumber = Column(String(12))\n",
      "language": "python"
    }
  ]
}
[/block]
# One to Many
SQLAlchemy defines one to many relationships by the following statement;

"A one to many relationship places a foreign key on the child table referencing the parent. `relationship()` is then specified on the parent, as referencing a collection of items represented by the child"

Consider a blog system where blog posts. This relation can be done in glim by the following;
[block:code]
{
  "codes": [
    {
      "code": "# models.py\nfrom glim_extentions.db import Model\nfrom sqlalchemy import Column, Integer, String, Text, ForeignKey\nfrom sqlalchemy.orm import relationship, backref\n\nclass Post(Model):\n\t\t__tablename__ = 'posts'\n\t\tid = Column(Integer, primary_key=True)\n\t\tcomments = relationship(\"Comment\")\n\nclass Comment(Model):\n\t\t__tablename__ = 'comments'\n\t\tid = Column(Integer, primary_key=True)\n\t\tpost_id = Column(Integer, ForeignKey('posts.id'))\n\t\ttext = Column(Text(300))",
      "language": "python"
    }
  ]
}
[/block]
You can achieve bidirectional relationship in Post using `backref` statement;
[block:code]
{
  "codes": [
    {
      "code": "class Post(Model):\n\t\t__tablename__ = 'posts'\n\t\tid = Column(Integer, primary_key=True)\n\t\tcomments = relationship(\"Comment\", backref='comments')",
      "language": "python"
    }
  ]
}
[/block]
This change will also provide a "reverse" type of one to many relationship namely "many to one".

## Many to One
SQLAlchemy defines many to one relationships by the following statement;

"Many to one places a foreign key in the parent table referencing the child. relationship() is declared on the parent, where a new scalar-holding attribute will be created"

Many to One relationships can be achieved with backref attribute. Consider a system where users can join a group. In this system, users are limited to join to only one group.
[block:code]
{
  "codes": [
    {
      "code": "# models.py\nfrom glim_extentions.db import Model\nfrom sqlalchemy import Column, Integer, String, Text, ForeignKey\nfrom sqlalchemy.orm import relationship, backref\n\nclass User(Model):\n    __tablename__ = 'user'\n    id = Column(Integer, primary_key=True)\n    group_id = Column(Integer, ForeignKey('group.id'))\n    group = relationship(\"Group\")\n\nclass Group(Model):\n\t\t__tablename__ = 'group'\n\t\tid = Column(Integer, primary_key=True)",
      "language": "python"
    }
  ]
}
[/block]
You can achieve bidirectional relationship in User using `backref` statement;
[block:code]
{
  "codes": [
    {
      "code": "class User(Model):\n    __tablename__ = 'user'\n    id = Column(Integer, primary_key=True)\n    group_id = Column(Integer, ForeignKey('group.id'))\n    group = relationship(\"Group\", backref=\"users\")",
      "language": "python"
    }
  ]
}
[/block]
## Many to Many
SQLAlchemy defines many to many relationships by the following statement;

"Many to Many adds an association table between two classes. The association table is indicated by the secondary argument to relationship(). Usually, the Table uses the MetaData object associated with the declarative base class, so that the ForeignKey directives can locate the remote tables with which to link"

Consider a system where blog posts have tags. This can be done by the following;
[block:code]
{
  "codes": [
    {
      "code": "# models.py\nfrom glim_extentions.db import Model\nfrom sqlalchemy import Column, Integer, String, Text, ForeignKey\nfrom sqlalchemy.orm import relationship, backref\n\nclass Post(Model):\n\t\t__tablename__ = 'posts'\n\t\tid = Column(Integer, primary_key=True)\n  \ttags = relationship('Tag', secondary=tags)\n\ntags = Table('tags',\n\tColumn('tag_id', Integer, ForeignKey('tag.id'),\n\tColumn('post_id', Integer, ForeignKey('post_id'))))\n\nclass Tag(Model):\n\t\tid = Column(Integer, primary_key=True)",
      "language": "python"
    }
  ]
}
[/block]
A bidirectional relationship could be established using `backref` in the Post model by the following;
[block:code]
{
  "codes": [
    {
      "code": "class Post(Model):\n\t\t__tablename__ = 'posts'\n  \tid = Columtn(Integer, primary_key=True)\n  \ttags = relationship('Tag', secondary=tags, backref='tags')",
      "language": "python"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Querying"
}
[/block]
After defining releations and model mappings, querying is the most important concept to learn. In glim, there are many ways to achieve querying. You can even write raw sql queries using the DB API. This section explains the orm querying part only.

# Switching between connections
In glim, it is said above that you can do connection aliasing in the "default-analytics" connection. You can switch to "analytics" database connection by the following;
[block:code]
{
  "codes": [
    {
      "code": "# app/config/<env>.py\nconfig = { \n\t\t'extensions': {\n    \t\t'db': {\n            'default': {\n                'driver': 'mysql',\n              \t'host': 'localhost',\n              \t'schema': 'test',\n              \t'user': 'root',\n              \t'password': '',\n        \t\t\t\t'orm': True\n          \t},\n      \t\t\t'analytics': {\n        \t\t\t\t'driver': 'mysql',\n\t\t\t\t\t\t\t  'host': 'localhost',\n\t\t\t\t\t \t\t\t'schema': 'analytics',\n\t\t\t\t\t \t\t\t'user': 'root',\n\t\t\t\t\t \t\t\t'password': '',\n        \t\t\t\t'orm': False\n        \t\t}\n      \t},\n    }\n}\n\nfrom glim_extentions.db import Orm\n\nOrm.connection('analytics').blah() # performs operations on the analytics database\nOrm.blah() # performs operations on the default conncetion",
      "language": "python"
    }
  ]
}
[/block]
In this example we can infer that Orm object holds the active session object of SQLAlchemy.

# The query() function
The `query()` function takes model classes or model column names and configures itself. You can use method chaining here by the following;
[block:code]
{
  "codes": [
    {
      "code": "from glim_extensions.db import Orm\n\n# query from a class\nOrm.query(User).filter_by(name='Aras').all()\n\n# querying from a class with only one row\nOrm.query(User).filter_by(name='Aras').first()\n\n# query with multiple classes\nOrm.query(User, Phone).join('phone').filter_by(name='Aras').all()\n\n# query specific columns\nOrm.query(User.name, User.id).all()",
      "language": "python"
    }
  ]
}
[/block]
You can get much more information in the [Querying](http://docs.sqlalchemy.org/en/rel_0_9/orm/query.html) section of SQLAlchemy.
[block:api-header]
{
  "type": "basic",
  "title": "Adding new items with orm session"
}
[/block]
You can create new models and persist in the database using `add()` & `commit()` function.
[block:code]
{
  "codes": [
    {
      "code": "# services.py\nfrom glim_extensions.db import Orm\nfrom app.models import User\n\ndef add_users(user1_name, user2_name):\n\t\tuser1 = User(name=user1_name)\n  \tuser2 = User(name=user2_name)\n  \tOrm.add(user1)\n  \tOrm.add(user2)\n  \tOrm.commit() # write changes to the database",
      "language": "python"
    }
  ]
}
[/block]
You can also add the users as a list by the following
[block:code]
{
  "codes": [
    {
      "code": "Orm.add_all([user1, user2])",
      "language": "python"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Disabled autocommit",
  "body": "Currently, it is required to write `commit()` statements to persist models to database. Although it is highly suggested to make these database changes transactional, the autocommit mode will be enabled. This issue will be addressed on the future releases."
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Deleting Items using orm session"
}
[/block]
You can filter and delete tuples by the following;
[block:code]
{
  "codes": [
    {
      "code": "from glim_extensions.db import Orm\n\nOrm.query(User).filter(id=7).delete()",
      "language": "python"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Using DB API"
}
[/block]
The DB API includes the `engine` instance of active connection. You can use SQL Expression Language that SQLAlchemy provides. It is highly recommended to read [this documentation](http://docs.sqlalchemy.org/en/rel_0_9/core/tutorial.html#) first.

# Running SQL Expression on models
The documentation above showed how to define models. SQL Expression language requires `Table` object to run chained sql statements. Consider an example of a system holding users in a model. The following example shows a simple select statemtent on defined model;
[block:code]
{
  "codes": [
    {
      "code": "# models.py\nfrom glim_extensions.db import Model\nfrom sqlalchemy import Column, Integer, String, Text, ForeignKey\nfrom sqlalchemy.orm import relationship, backref\n\nclass User(Model):\n\t\t__tablename__ = 'users'\n  \tid = Column(Integer, primary_key=True)\n  \tname = Column(String(250))\n\n# services.py\nfrom app.models import User\nfrom glim.db import Database as DB\nfrom sqlalchemy.sql import select\n\ndef select_all_users():\n\t\ts = select([User.__table__])\n  \tresult = DB.execute(s)\n  \tfor row in result:\n\t\t    print row\n  ",
      "language": "python"
    }
  ]
}
[/block]
You can notice that sql expression language can be run by using `__table__` property of a model. You can fetch one row using `fetchone()` function;
[block:code]
{
  "codes": [
    {
      "code": "# services.py\nfrom glim_extensions.db import Database as DB\nfrom app.models import User\n\n# get rows using defined column names\ndef select_first_user():\n\t\ts = select([User.__table__])\n  \tresult = DB.execute(s)\n  \trow = result.fetchone()\n  \tprint(\"name:\", row['name'], \"; fullname:\", row['fullname'])\n  \n# get rows using column name in __table__\ndef select_first_user2():\n  \ts = select([User.__table__])\n  \tresult = DB.execute(s)\n  \trow = result.fetchone()\n  \tprint(\"name:\", row[User.__table__.c.name],\n        \t\"; fullname:\", row[User.__table__.c.fullname])",
      "language": "python"
    }
  ]
}
[/block]

# Insert Expressions
Consider the example of the user model above. You can achieve insert expressions by running `Model.__table__` statement by the following;
[block:code]
{
  "codes": [
    {
      "code": "# services.py\nfrom app.models import User\nfrom glim_extensions.db import Database as DB\n\ndef insert_user(name):\n\t\tins = User.__table__\n  \t\t\t\t\t\t.insert().values(name='Aras')\n \t\tresult = DB.execute(ins)\n  \treturn result",
      "language": "python"
    }
  ]
}
[/block]
# Select Expressions
You can run selects by chaining statements;
[block:code]
{
  "codes": [
    {
      "code": "from glim_extensions.db import Database as DB\nfrom app.models import User, Address\n\nusers = User.__table__\naddresses = Address.__table__\n\n# an example of chaining multiple tables\ns = select([users, addresses])\n\t\t\t.where(users.c.id == addresses.c.user_id)\n  \n# an example of chaining more and or_ statements\ns = select([users.c.fullname + \", \" + addresses.c.email_address])\n\t\t\t.where(users.c.id == addresses.c.user_id)\n  \t\t.where(users.c.name.between('m', 'z'))\n    \t.where(\n       \tor_(\n        \taddresses.c.email_address.like('%@gmail.com')\n         \taddresses.c.email_address.like('%@yandex.com')\n        ) \n      )\n      \nrows = DB.execute(s).fetchall()",
      "language": "python"
    }
  ]
}
[/block]
# Order By, Group By
[block:code]
{
  "codes": [
    {
      "code": "from sqlalchemy import func, desc\nfrom glim_extensions.db import Database as DB\nfrom app.models import Address\n\naddresses = Address.__table__\n\nstmt = select([\n\taddresses.c.user_id,\n\tfunc.count(addresses.c.id).label('num_addresses')]).\\\n\torder_by(\"num_addresses\")\n\nDB.execute(stmt).fetchall()\n\nstmt = select([\n\t\t\t\taddresses.c.user_id,\n\t\t\t\tfunc.count(addresses.c.id).label('num_addresses')]).\\\n\t\t\t\torder_by(desc(\"num_addresses\"))\n  \nDB.execute(stmt).fetchall()",
      "language": "python"
    }
  ]
}
[/block]

# Update Expressions
You can run update statements using `update()` function by the following;
[block:code]
{
  "codes": [
    {
      "code": "from glim_extensions.db import Database as DB\nfrom app.models import User\n\nusers = User.__table__\n\nstmt = users.update().\\\n\t\t\t\twhere(users.c.name == 'Aras').\\\n\t\t\t\tvalues(fullname=\"Fullname: \" + users.c.name)\n\nresult = DB.execute(stmt)",
      "language": "python"
    }
  ]
}
[/block]
# Delete Expressions
You can run delete statements using `delete()` function by the following;
[block:code]
{
  "codes": [
    {
      "code": "from glim_extensions.db import Database as DB\nfrom app.models import Address\n\naddresses = Address.__table__\nDB.execute(addresses.delete())",
      "language": "python"
    }
  ]
}
[/block]

# Raw Queries
There are many ways of running raw sql queries 
[block:code]
{
  "codes": [
    {
      "code": "from sqlalchemy.sql import text\nfrom glim_extensions.db import Database as DB\n\ns = text(\n\t\t\"SELECT users.fullname || ', ' || addresses.email_address AS title \"\n\t\t\"FROM users, addresses \"\n\t\t\"WHERE users.id = addresses.user_id \"\n\t\t\"AND users.name BETWEEN :x AND :y \"\n\t\t\"AND (addresses.email_address LIKE :e1 \"\n\t\t\"OR addresses.email_address LIKE :e2)\")\n\nDB.execute(s, x='m', y='z',\n           e1='%@gmail.com',\n           e2='%@yandex.com')\n\t.fetchall()\n  \nsql = \"INSERT INTO users (full_name, title)\" \\\n      \"VALUES ('%s','%s')\" % (full_name, title))\"\n\nresult = DB.execute(sql)\nprint result",
      "language": "python"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Migrations"
}
[/block]
Migrations are currenlty available by [RDB Migrations](doc:rdb-migrations) extension. You can check the docs about database migrations there.