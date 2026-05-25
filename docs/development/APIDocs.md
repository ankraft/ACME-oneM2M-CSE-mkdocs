# API Docs

## Online API Documentation

The full API documentation for the ACME CSE is available online at [https://api.acmecse.net](https://api.acmecse.net){target="_new"}.

## Generating API Documentation


### Installing Pydoctor

Before running the commands, make sure to install the [pydoctor](https://github.com/twisted/pydoctor){target="_blank"} package, which is required to generate the API documentation. You can install it by running:

```bash title="Installing API Documentation Dependencies"
pip install pydoctor
```

### Generating API Documentation

You can generate the API documentation locally by running the following commands from the *tools/apidocs* directory of the ACME CSE distribution:

```bash title="Generating API Documentation"
make		# Generate API documentation
make open	# Open API documentation in the default web browser
```

The generated API documentation will be saved in the *docs/apidocs* directory of the ACME CSE distribution. You can open the generated API documentation by running `make open` or by opening the *index.html* file in the *docs/apidocs* directory in your web browser.