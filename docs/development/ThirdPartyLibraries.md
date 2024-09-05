# Third Party Components

The following third-party components are used by the ACME CSE.

## Core CSE
- The [cachetools](https://github.com/tkem/cachetools/){target=_new} package provides caching utilities. MIT License.
- The [cbor2](https://github.com/agronholm/cbor2){target=_new} package is used to parse and create CBOR serializations. MIT License.
- [InquirerPy](https://github.com/kazhala/InquirerPy/){target=_new} is a collection of common interactive command-line interfaces. MIT License.
- The [isodate](https://github.com/gweis/isodate){target=_new} package is used to parse and handle ISO 8601 time, date, and duration. BSD License.
- The [plotext](https://github.com/piccolomo/plotext){target=_new} library offers functions to plot graphs in the text console. MIT License.
- [rdflib](https://github.com/RDFLib/rdflib){target=_new} is a Python library for working with RDF. BSD 3-Clause License.
- The CSE uses the [Rich](https://github.com/willmcgugan/rich){target=_new} text formatter library to format various terminal output. MIT License. 
- [shapely](https://github.com/shapely/shapely){target=_new} is a library for manipulation and analysis of geometric objects. BSD 3-Clause License. 


## Connectivity
- For the CoAP protocol binding implementation the ACME CSE uses a fork of the [coapthon3](https://github.com/Tanganelli/CoAPthon3){target=_new} library. MIT Licsense.  
The fork is available on GitHub as [CoAPthon3-ACME-CSE](https://github.com/ankraft/CoAPthon3-ACME-CSE){target=_new} and on PyPi as [coapthon3-acme-cse](https://pypi.org/project/CoAPthon3-ACME-CSE/){target=_new}.
- The CSE uses the [Flask](https://flask.palletsprojects.com/){target=_new} web framework to service http(s) requests. BSD 3-Clause License.
- [flask-cors](https://github.com/corydolphin/flask-cors/){target=_new} is a *Flask* extension for handling Cross Origin Resource Sharing (CORS), making cross-origin AJAX possible.
- The [paho-mqtt](https://www.eclipse.org/paho/){target=_new} library provides a client class which enables applications to connect to an MQTT broker. Eclipse Public License 1.0 .
- The CSE uses the [Requests](https://requests.readthedocs.io){target=_new} HTTP Library to send requests vi http. Apache2 License
- [waitress](https://github.com/Pylons/waitress){target=_new} is a production-quality pure-Python WSGI server with very acceptable performance. ZPL 2.1 License.


## Database 
- [Psycopg](https://www.psycopg.org){target=_new} is a PostgreSQL adapter for the Python programming language. GNU Lesser General Public License.
- To store resources the CSE uses the lightweight [TinyDB](https://github.com/msiemens/tinydb){target=_new} document database. MIT License.


## Text UI
- [Textual](https://github.com/textualize/textual){target=_new} is a Rapid Application Development framework for to build textual user interfaces in Python. MIT License.
- [pyperclip](https://github.com/asweigart/pyperclip){target=_new} is a cross-platform Python module for copying and pasting text to the clipboard. BSD-3-Clause License.


## Web UI
- TreeJS: [https://github.com/m-thalmann/treejs](https://github.com/m-thalmann/treejs){target=_new}, MIT License.
- Picnic CSS : [https://picnicss.com](https://picnicss.com){target=_new}, MIT License.
