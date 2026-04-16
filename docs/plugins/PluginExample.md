# Developing an Example Plugin

The programming language for plugins is Python, the same as for the ACME CSE itself. 

To create a new plugin, one can follow the steps below. We will create a simple example plugin that will log a message when the CSE starts up and shuts down.

## Creating a Plugin Module

Create a new file for the Python module in the `plugins` folder of your ACME CSE's working directory. That is the directory from which the CSE is started.

For example, if your plugin is named `HelloWorld`, create a file named `HelloWorld.py` in the `plugins` directory.

User-defined plugins are loaded under the `plugins` Python module namespace.


## Writing the Plugin Code

In your plugin module, you need to define a class that is decorated with `@PluginManager.runnableClass`. This class will contain methods that are decorated to hook into the CSE's lifecycle events.

```python title="HelloWorld.py" linenums="1"
from acme.runtime import PluginManager
from acme.runtime.Logging import Logging

@PluginManager.plugin
class HelloWorldPlugin:

    @PluginManager.init
    def init(self) -> None:
        Logging.log('HelloWorld plugin initialized')

    @PluginManager.finish
    def finish(self) -> None:
        Logging.log('HelloWorld plugin finished')

    @PluginManager.start
    def start(self) -> None:
        Logging.log('Hello, world!')

    @PluginManager.stop
    def stop(self) -> None:
        Logging.log('Goodbye, world!')
```

In this example, the `HelloWorld` class is decorated with `@PluginManager.plugin`, indicating that it is a plugin class. There can only be one plugin class per plugin module. This class will automatically be instantiated by the `PluginManager` when the plugin is loaded.

The class has four methods that are decorated to hook into the CSE's lifecycle events: `init`, `finish`, `start`, and `stop`. Each method logs a message to the ACME logging system when the CSE starts up and shuts down.


## Running the Plugin

To run the plugin, start or restart the ACME CSE from within the working directory containing your `plugins` folder. You should see log messages indicating that the plugin has been loaded, and the class has been instantiated, initialized, and started. When you shutdown the CSE, you should see log messages indicating that the plugin has been stopped and finished.

The log output should look similar to the following:

```text title="ACME CSE Log Output"
...
INFO    MainThread - HelloWorld plugin initialized              HelloWorld.py:9
INFO    MainThread - Hello, world!                              HelloWorld.py:17
...
INFO    MainThread - Goodbye, world!                            HelloWorld.py:21
INFO    MainThread - HelloWorld plugin finished                 HelloWorld.py:13
...
```
