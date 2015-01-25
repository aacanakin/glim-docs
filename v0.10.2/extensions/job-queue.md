# glim-jobqueue - a redis jobqueue extension for glim

This is a jobqueue implementation for glim framework. It uses [glim-redis](doc:redis). extension for redis connection. You can use job extension for async processes that consume much more time than a typical request. For instance, it can be done mail queue, push notifications, etc. as jobs.

# Installation
- First, install [glim-redis](doc:redis) extension
- Clone the github repo, move job folder to ext folder
- Remove `.git` directory.
[block:code]
{
  "codes": [
    {
      "code": "# install glim-extensions from pip\npip install glim-extensions",
      "language": "shell"
    }
  ]
}
[/block]
# Configuration
- Append job configuration just after gredis configuration in your config file
[block:code]
{
  "codes": [
    {
      "code": "# app/config/<env>.py\nconfig = {\n    'extensions' : {\n        'gredis' : {\n            'default' : {\n                'host' : 'localhost',\n                'port' : '6379',\n                'db'   : 0\n            }\n        },\n        'job' : {\n            'default' : {\n                'redis'  : 'default',\n                'jobs'   : 'jobs',    # redis list name for jobs\n                'failed' : 'failed',  # redis list name for failed jobs\n            }\n        },\n    },\n    # ...\n}",
      "language": "python"
    }
  ]
}
[/block]
# Initializing Job Extension
To initialize job extension, type the following;
[block:code]
{
  "codes": [
    {
      "code": "glim job:init\n# this will create a job.py on your app folder",
      "language": "shell"
    }
  ]
}
[/block]
# Creating Jobs
[block:code]
{
  "codes": [
    {
      "code": "# app/jobs.py\nfrom glim import Log\nfrom glim_extensions.job import Job\n\nclass HelloJob(Job):\n    def run(self): # self.data is registered when a Job \n        if 'author' in self.data.keys():\n            Log.info('hello %s' % self.data['author'])\n        else:\n            Log.info('hello glim!')",
      "language": "python"
    }
  ]
}
[/block]
# Producing Jobs
[block:code]
{
  "codes": [
    {
      "code": "from glim_extensions.job import JobQueue\ndata = {\n    'author' : 'Aras Can Akin'\n}\nJobQueue.push(HelloJob(data)) # returns True or False\nJobQueue.push(HelloJob)",
      "language": "python"
    }
  ]
}
[/block]
# Consuming Jobs
[block:code]
{
  "codes": [
    {
      "code": "glim job:consume --name jobs\n# output\n# hello Aras Can Akin\n# hello",
      "language": "shell"
    }
  ]
}
[/block]
# Failed Jobs
In jobqueue extension, the failed jobs can be moved to an another redis list by raising exceptions. The `$ glim job:consume` command is looking for jobs to throw exception if a failed job occurs.
[block:code]
{
  "codes": [
    {
      "code": "# app/jobs.py\nfrom glim import Log\nfrom glim_extensions.job import FailedJobError\nfrom glim_extensions.job import Job\n\nclass HelloJob(Job):\n    def run(self):\n        if 'author' in self.data.keys():\n            Log.info('hello %s' % self.data['author'])\n        else:\n            raise FailedJobError()",
      "language": "python"
    }
  ]
}
[/block]
In this example, when you push a job without author, the job will go to failed list in redis

# Roadmap
- the job system should work for many other message queue services like AWSQ, RabbitMQ, IronMQ, etc.
- command for flushing job queue
- add feature to push/pop scheduled jobs