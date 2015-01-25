Glim framework source resides in pypi which is the most popular python repository service. You can download glim and create a simple app by the following;
[block:code]
{
  "codes": [
    {
      "code": "# create a project folder\nmkdir project\ncd project\n\n# create & activate virtualenv\nvirtualenv venv\n. venv/bin/activate\n\n# install glim from pypi\npip install glim\n\n# prints glim help\nglim --help\n\n# create a new app\nglim new\n\n# start the app\nglim start\n",
      "language": "shell"
    }
  ]
}
[/block]
If you don't have virtualenv & pip you can install them by the following;
[block:code]
{
  "codes": [
    {
      "code": "# install pip\nsudo easy_install pip\n\n# install virtualenv\nsudo easy_install virtualenv\n# OR\nsudo pip install virtualenv",
      "language": "shell"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "virtualenv",
  "body": "It's completely optional to use virtualenv. However, to make the app work seperated from project, glim changes stuff in python system path. Therefore, it's highly recommended to use it."
}
[/block]
Optionally, if you want multiple apps to run with the same glim, obviously you don't need to install it on seperate virtual environments. You can also create & start a glim app by the following;
[block:code]
{
  "codes": [
    {
      "code": "# create a project in project folder\nglim new project\ncd project\n\n# start the project\nglim start",
      "language": "shell"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "cwd path",
  "body": "Glim currently requires to be run with the current working directory path. Therefore, `$ glim start project` won't run the app. It's needed to be run **inside** project folder.\n\nThis issue will be addressed in the further releases of project"
}
[/block]