The concept of Facade is there to hide abstractions from core classes. In glim, facades can be used to hold single instances of objects that can be used everywhere during the runtime.

Consider an example of a class namely "Database". A Database class example would be the following;
[block:code]
{
  "codes": [
    {
      "code": "# database.py\nclass Database:\n\n  \tdef __init__(self, config):\n      \tself.config = config\n        self.connection = None\n  \n  \tdef connect(self):\n\t\t\t\t# perform database connection using config\n        self.connection = connect_to_db()\n    \n    def query(sql):\n      \t# perform database query execution using the db connection\n        return self.connection.execute_sql(sql)\n      \t",
      "language": "python"
    }
  ]
}
[/block]
This example is a primitive db connection library, so it won't reflect the real world. It is just given to teach the concept of Facades. In this example, you can instantiate an object of Database by the following;
[block:code]
{
  "codes": [
    {
      "code": "# some_script.py\nfrom database import Database\n\nconfig = {\n\t\t'host': 'localhost',\n    'port': 3306,\n  \t'user': 'root',\n  \t'password': 'pwd'\n}\n\ndb = Database(config)\ndb.connect()\n\nsql = \"SELECT 1 FROM test.hello\"\n\nprint(db.query(sql))",
      "language": "python"
    }
  ]
}
[/block]
It might be noticed that a database instance could be used when the web server starts and used everywhere in model or service layer. Facades can provide this type of abstraction. So, let's make a Database Facade;
[block:code]
{
  "codes": [
    {
      "code": "from database import Database\nfrom glim.core import Facade\n\nclass DB(Facade):\n  \taccessor = Database",
      "language": "python"
    }
  ]
}
[/block]
A typical Facade has two functions namely `register` and `boot`. The register function can inject a configuration dict to the accessor object, in this example, namely Database class. The `boot` function can inject `(*args, **kwargs)` which can be anything. Therefore, glim can't understand how to boot an object. However, it can automatically use register function with config passed. Glim can boot these objects by the following;
[block:code]
{
  "codes": [
    {
      "code": "# create the database instance, holds it statically\nDB.register(config)\n\n# create the database instance with multiple arguments\nDB.boot(*args, **kwargs)",
      "language": "python"
    }
  ]
}
[/block]
By default, glim has a number of facades. These facades are the following;
[block:parameters]
{
  "data": {
    "h-0": "Facade name",
    "h-1": "The mapped accessor",
    "0-0": "glim.facades.Config",
    "0-1": "glim.core.Config",
    "1-0": "glim.facades.View",
    "1-1": "glim.component.View",
    "2-0": "glim.facades.Log",
    "2-1": "glim.log.Log",
    "3-0": "glim.facades.Database",
    "3-1": "glim.db.Database",
    "4-0": "glim.facades.Orm",
    "4-1": "glim.db.Orm"
  },
  "cols": 2,
  "rows": 5
}
[/block]
These mappings provide the mapped accessors to be instantiated in the runtime first and use them everywhere.
[block:callout]
{
  "type": "warning",
  "title": "No singletons",
  "body": "Again, this pattern is not a singleton pattern. There exists singleton manner but the implementation is completely different. Singletons mostly use private constructors which is a bad practice."
}
[/block]