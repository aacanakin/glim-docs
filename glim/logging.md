Glim has logging feature using python's standart `logging` module. You can configure logging by the `log` key of configuration;
[block:code]
{
  "codes": [
    {
      "code": "# app/config/<env>.py\nconfig = {\n  # ...\n\n  'log' : {\n\t\t'level' : 'info',\n\t\t'format' : '[%(levelname)s] : %(message)s',\n\t\t'file' : 'app/storage/logs/debug.log'\n\t},\n\n  # ...\n}",
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
    "2-1": "STD_OUT"
  },
  "cols": 2,
  "rows": 3
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
      "code": "from glim.facades import Log\n\n# log info level\nLog.info('this is info') \n\n# log warning level\nLog.warning('this is warning')\n\n# log debug level\nLog.debug('this is debug')\n\n# log error level\nLog.error('this is error')\n\n# log critical level\nLog.critical('this is critical')",
      "language": "python"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Optional not colored logs",
  "body": "Currently, it is limited to use logs with no color. However, this issue will be addressed on the future releases."
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "App, Root and Framework Loggers",
  "body": "Currently, there exists only 1 logger. In the future releases of glim, there will be app, root and framework level loggers. This issue will be addressed on the future releases."
}
[/block]