In glim, there is a command line utility layer where you can define command line apps that can be run from terminal. In a typical glim app, all command line apps reside in `app/commands.py`. In this file, all commands are extended from `Command` class in `glim.command` module.

This command layer uses `ArgumentParser` object of `argparse` module. Consider the examples of Hello & Greet commands. Hello command just prints "Hello World!" and Greet command takes an user name argument prints "hello :name";
[block:code]
{
  "codes": [
    {
      "code": "# commands.py\nfrom glim.command import Command\n\nclass HelloCommand(Command):\n  \tname = 'hello'\n    description = 'prints hello world'\n\t\tdef configure(self):\n\t\t\t\tpass\n \t\tdef run(self):\n      \tprint(\"Hello World!\")\n\t\t    pass\n      \nclass GreetCommand(Command):\n  \tname = 'greet'\n    description = 'greets a user'\n    def configure(self):\n\t\t\t\tself.add_argument(\"name\", nargs='?',\n                          help=\"enter project name\",\n                          default=None)\n    def run(self):\n      \tprint(\"Hello %s!\" % self.args.name)\n  ",
      "language": "python"
    }
  ]
}
[/block]
These commands are automatically registered when you run `glim` from command line. The following outputs will be appeared on glim commands;
[block:code]
{
  "codes": [
    {
      "code": "$ glim hello\nHello World!\n\n$ glim greet aras\nHello Aras!",
      "language": "shell"
    }
  ]
}
[/block]
In a typical Command, `self.add_argument(*args, **kwargs)` would create an argument using `ArgumentParser`. You can see more examples of argument adding [here](https://docs.python.org/2/library/argparse.html). 
[block:callout]
{
  "type": "warning",
  "title": "Register arguments in only configure function",
  "body": "You can only register arguments inside the configure function."
}
[/block]