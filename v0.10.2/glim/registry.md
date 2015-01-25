In glim, most of the core components are extending from Registry class. The Registry class provides configuration which can be dynamically accessed using "." notation. The Registry implementation is the following;
[block:code]
{
  "codes": [
    {
      "code": "class Registry:\n\n    \"\"\"\n\n    This class is basically a dictionary supercharged with\n    useful functions.\n\n    Attributes\n    ----------\n      registrar (dict): A registrar to hold a generic\n        configuration of a class.\n\n    Usage\n    -----\n      config = {\n        'a' : 'b',\n        'c' : {\n            'd' : 'e'\n        }\n      }\n      reg = Registry(config)\n      reg.get('a') # returns b\n      reg.get('c.d') # returns e\n      reg.set('f', 'g') # sets 'f' key of dict\n      reg.get('f') # returns g\n\n    \"\"\"\n\n    def __init__(self, registrar):\n        self.registrar = registrar\n\n    def get(self, key):\n        \"\"\"\n\n        Function deeply gets the key with \".\" notation\n\n        Args\n        ----\n          key (string): A key with the \".\" notation.\n\n        Returns\n        -------\n          reg (unknown type): Returns a dict or a primitive\n            type.\n\n        \"\"\"\n        try:\n            layers = key.split('.')\n            value = self.registrar\n            for key in layers:\n                value = value[key]\n            return value\n        except:\n            return None\n\n    def set(self, key, value):\n        \"\"\"\n\n        Function deeply sets the key with \".\" notation\n\n        Args\n        ----\n          key (string): A key with the \".\" notation.\n          value (unknown type): A dict or a primitive type.\n\n        \"\"\"\n        target = self.registrar\n        for element in key.split('.')[:-1]:\n            target = target.setdefault(element, dict())\n        target[key.split(\".\")[-1]] = value\n\n    def flush(self):\n        \"\"\"\n\n        Function clears the registrar\n\n        Note:\n          After this function is called, all of your data\n          in your registry will be lost. So, use this smartly.\n\n        \"\"\"\n        self.registrar.clear()\n\n    def update(self, registrar):\n        \"\"\"\n\n        Function batch updates the registry. This function is an\n        alias of dict.update()\n\n        Args\n        ----\n          registrar (dict): A dict of configuration.\n\n        Note:\n          After this function is called, all of your data may be\n          overriden and lost by the registrar you passed. So, use\n          this smartly.\n\n        \"\"\"\n        self.registrar.update(registrar)\n\n    def all(self):\n        \"\"\"\n\n        Function returns all the data in registrar.\n\n        Returns\n        -------\n          registrar (dict): The current registry in the class.\n\n        \"\"\"\n        return self.registrar\n\n    def __str__(self):\n        return self.registrar\n",
      "language": "python"
    }
  ]
}
[/block]
You can see the docstring introduces the usage of a registry object. In glim, the Config class is extending the registry. It has the following implementation;
[block:code]
{
  "codes": [
    {
      "code": "class Config(Registry):\n\n    \"\"\"\n\n    The configuration class is to hold framework level constants.\n    It holds the dict in app.config.<env>.config.\n\n    \"\"\"\n",
      "language": "python"
    }
  ]
}
[/block]
You can notice that there aren't any changes in the Registry class. The Config can be instantiated with a dict namely the registrar.