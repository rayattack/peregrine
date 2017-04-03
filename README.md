Peregrine
=========

[![Build Status](https://travis-ci.org/ffireio/ffire-python.svg?branch=dev)](https://travis-ci.org/ffireio/ffire-python)

Peregrine is a convention opinionated python web framework. 
It enables you to create extremely powerful web applications with little worry about complexity and has an extensive
html, xml, and json rendering library for building web applications with ease.

Peregrine installation is fairly straight forward. Python2.6 and above is required and installation can be easily
completed by using the appropriate version pip command.

## Installation

```shell

pip install peregrine

```

## Minimal Peregrine Application Example

```python


from peregrine.interface import IController
from peregrine.interface import IRouter
from peregrine.repository import Repository
from peregrine.configuration import Configuration
from peregrine.application import Application
from peregrine.constants import *


class User(Repository):
    pass


class UserController(IController):

    def handle_post(request, response, model):
        """
        """
        model.load_from_json(request.json)
        model.load_from_xml(request.xml)
        model.load_from_form(request.form)
        model.save()
        response.json = model.to_json()

    def handle_put(request, model):
        pass

    def handle_get(request, response, model):
        response.json = model.to_json()
        response.context = model.to_context()
        response.xml = model.to_xml()


class UserView(IRouter):
    __route__ = "/users"

    __required__ = {
        "email": {LENGTH: (6, 75), HAS_CAPS: True, HAS_SYMBOLS: ('@', '.')},
        "password": {HAS_CAPS: True, HAS_SYMBOLS: True},
        "repeat_password": {}
    }


if __name__ == "__main__":
    configuration = Configuration({
        "port": 3000,
        "adapter": "mongodb",  #: others include mssql, mysql, oracle, postgres, mariadb
        "debug": True,
        "logging": "application.log"
    })
    application = Application(configuration)
    application.run()



```


## Conventional Structure of Peregrine Applications

```shell

top_level_directory/
---- __init__.py
---- controllers/
-------- __init__.py
-------- user_controller.py
-------- event_controller.py
---- models/
-------- __init__.py
-------- user.py
-------- event.py
---- views/
-------- __init__.py
-------- user_view.py
-------- event_view.py
---- templates/
-------- index.html
---- static/
-------- imgs/
------------ icon.ico
-------- css/
------------ site.css
-------- js/
------------ site.js

```

It is possible to use nested packages in the controllers, models, and views packages in place of python modules. i.e. ```/user/__init__.py``` in place of the corresponding
controller python module ```user_controller.py``` as long as nested python modules follow the same controllers, models, and views naming conventions.
