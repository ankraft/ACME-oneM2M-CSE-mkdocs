# Installing ACME on a Raspberry Pi

Doing installations on a Raspberry Pi, especially on older versions, could be a small challenge, because some of the external Python packages require extra libraries that are not necessarily available on an old Raspberry Pi.

This guide presents the necessary steps to install Python 3.11, the external Python packages and ACME on an older Raspberry Pi 3B with Raspberry Pi OS (32 bit).

## Installation for Raspberry Pi OS "Bookworm" or newer

Newer Raspberry Pi OS versions come already with a Python 3.11.x version installed. It is still recommended to install [pyenv](https://github.com/pyenv/pyenv)(target=new} or another virtual environment to keep the system installation clean.


### Installing pyenv

The easiest way to install *pyenv* is to run the automatic installer as described [here](https://github.com/pyenv/pyenv#automatic-installer){target=new}:

```bash title="pyenv Automatic Installation"
curl https://pyenv.run | bash
```

After this installation is finished it is recommended to follow the on-screen instructions to update the *.bashrc* file to always use and initialize the virtual environments.

### Installing a Different Python Version

PyEnv can be used to install a different Python version. This usually requires a couple of system libraries that need to be installed before.

See also [How to Install pyenv](HowTo-pyenv.md) for further details.

#### Installing Extra Libraries and Components 

```sh title="Install Extra Components and Libraries"
sudo apt update
sudo apt-get install -y build-essential tk-dev libncurses5-dev libncursesw5-dev libreadline6-dev libdb5.3-dev libgdbm-dev libsqlite3-dev libssl-dev libbz2-dev libexpat1-dev liblzma-dev zlib1g-dev libffi-dev libatlas-base-dev libgeos-dev gfortran git cmake libpq-dev
```

#### Next Steps

One can now continue with the [installation and activation of a different Python version in the virtual environment](HowTo-pyenv.md) and the [installation of ACME itself](../setup/Installation.md).

## Installation for older Raspberry Pi OS Versions

This section describes the installation process on older (also 32bit) versions of the Raspberry PI OS (or Raspbian).

### Installing Python and Tools

First, we need to install a newer Python 3 runtime on our Raspberry Pi.

#### Downloading Python Source

The following download gets the source code from the official Python repository. It could be a newer version of Python as well, of course.

```sh title="Download Python"
wget https://www.python.org/ftp/python/3.11.4/Python-3.11.4.tgz
```

#### Installing Extra Libraries and Components

The following commands install the necessary system libraries and other tools to compile Python on the Raspberry Pi. :

```sh title="Install Extra Components and Libraries"
sudo apt update
sudo apt-get install -y build-essential tk-dev libncurses5-dev libncursesw5-dev libreadline6-dev libdb5.3-dev libgdbm-dev libsqlite3-dev libssl-dev libbz2-dev libexpat1-dev liblzma-dev zlib1g-dev libffi-dev libatlas-base-dev libgeos-dev gfortran git cmake libpq-dev
```

#### Compile Python

The next step is to unpack and to unpack, configure, make and install the Python runtime.

```sh title="Compile Python"
tar -xzvf Python-3.11.4.tgz 
cd Python-3.11.4/
./configure --enable-optimizations
sudo make -j 4
sudo make altinstall
cd ..
```

### Install, Configure, and Run ACME

Next, we will install the ACME CSE. 

You can now follow the instructions in the [Installation](../setup/Installation.md) guide to install the ACME CSE on your Raspberry Pi.



 