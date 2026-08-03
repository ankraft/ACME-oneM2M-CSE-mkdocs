# Debug Mode

The CSE tries to catch errors and give helpful advice as much as possible during runtime.
However, there are circumstances when this could not done easily, e.g. during the startup of the Python interpreter. 

Another situation is when the CSE is started in a debugger, e.g. in Visual Studio Code, or with the Python debugger ([pdb](https://docs.python.org/3/library/pdb.html){target=_new}). In these situations the CSE console input might interfere with the debugger input, which makes it difficult to use the debugger. In debug mode the console input is disabled, so that the debugger can be used without interference. The console will still be displayed, but it will not accept any input, except for the `Ctrl+C` key combination to stop the CSE.

## Enabling Debug Mode

In order to provide additional information in these situations one can set the *ACME_DEBUG* environment variable (to any value):

=== "bash"
	```sh title="Set the ACME_DEBUG environment variable"
	export ACME_DEBUG=1
	```
=== "fish"
	```sh title="Set the ACME_DEBUG environment variable"
	set -x ACME_DEBUG 1
	```

Then run the CSE as usual. 

## Disabling Debug Mode
To disable the debug mode for the next run, simply unset the environment variable:

=== "bash"
	```sh title="Unset the ACME_DEBUG environment variable"
	unset ACME_DEBUG
	```
=== "fish"
	```sh title="Unset the ACME_DEBUG environment variable"
	set -e ACME_DEBUG
	```
