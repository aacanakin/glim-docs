Glim has logging feature using python's standart `logging` module. In glim, there exists 2 types(instances) of logging module. One of them is the "app" logger that is used inside web application flow. The other one is called "glim" logger which is used internally inside the framework. You can configure logging by the `log` key of configuration;
[block:code]
{
  "codes": [
    {
      "code": "# app/config/<env>.py\nconfig = {\n  # ...\n\t'log' : {\n  \t\t'app' : {\n          'level': 'info',\n          'format': '[%(levelname)s] - application : %(message)s',\n          'colored': True,\n          'file' : 'app/storage/logs/app.log'\n      },\n    \t'glim' : {\n          'level' : 'info',\n          'format' : '[%(levelname)s] : %(message)s',\n          'colored': True\n          'file' : 'app/storage/logs/glim.log'  \n    \t},\n  },\n  # ...\n}",
      "language": "python"
    }
  ]
}
[/block]
This dictionary means that the logging will be done in "info" level and the format will be like 
"[INFO] : msg". `file` key stands for the file path of the log file. These keys have default values. If you don't provide a particular key, the default one will be configured. Here is a mapping of the default keys;
[block:parameters]
{
  "data": {
    "h-0": "key",
    "0-0": "level",
    "h-1": "default value",
    "0-1": "debug",
    "1-0": "format",
    "1-1": "\"%(message)s\"",
    "2-0": "file",
    "2-1": "STD_OUT",
    "3-0": "colored",
    "3-1": "True"
  },
  "cols": 2,
  "rows": 4
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Usage"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "from glim import Log\n\n# write a colorless string in debug level\nLog.write('this is a log without color in debug level')\n\n# log info level\nLog.info('this is info') \n\n# log warning level\nLog.warning('this is warning')\n\n# log debug level\nLog.debug('this is debug')\n\n# log error level\nLog.error('this is error')\n\n# log critical level\nLog.critical('this is critical')",
      "language": "python"
    }
  ]
}
[/block]