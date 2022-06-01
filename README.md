# Bot
The official StudSec bot written in Python.

## Installation
For installation, git clone this repository (or download and unzip). From there
simply run
```shell
pip3 install -r requirements.txt
```
And you should be all set!
## Usage
To run the bot first check `config.py` to see what sensitive information needs to
be set.

In all cases you'll need to set the discord token:
```shell
export DISCORD_TOKEN=OTg..
```

Once this is all set, simply run `main.py`
```shell
python3 main.py
```

*Note, for security reasons the environment variables will be removed at runtime.
This means they need to be set everytime.*
## Documentation
If you want to add new functionality to the bot it is recommended to follow this 
template.
```python
"""
Module description
"""
# Custom imports
from . import registry

# External imports
import example


class ModuleName:
    def __init__(self, client):
        self.commands = {
            "hello": self.hello,
        }
        self.name = ["Module name"]
        self.category = ["Module Category"]
        self.client = client
        self.type = "Runtime"

    async def process_message(self, message):
        if message.content.split(" ")[0][1:] in self.commands.keys():
            return self.commands[message.content.split(" ")[0][1:]](message)

    @staticmethod
    def hello(msg):
        return "Hello World."

registry.register(ModuleName)
```
A few notes about the bot api:
- Upon loading, the module will be given the client object as a parameter. This is done
to allow for messages to be sent in different contexts. In the future this might
become a privileged item.
- For a module to work it must 1) be in `__init__.py` and 2) be registered using `registry.py`
- You can store configuration information within `config.py`, keep in mind that the
dictionary key must match the **class** name (`__name__`), not the module name. The
config dictionary of your module will then be passed as an additional argument, so
account for that.
- The first item in the self.name list is used, the rest are considered synonyms.
- Currently, a module can be of two types, `Runtime` and `Runtime-Privileged`. The
latter gets access to the other modules. Please only use this if absolutely necessary,
also expect some changes to this.

## TODO
- Bot
  - Add load/reload/unload functionality
  - Add module context isolation
- Modules
  - Browser for client side attacks
  - CTF scoreboard
  - Top 5 role