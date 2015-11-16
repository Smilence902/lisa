# Introduction

This project provides a collection of tools to support regression testing and
interactive analysis of workload behavior. Its main goal is to support Linux
kernel developer to assess the impact of modification in core parts of the
kernel. The main focus is on scheduler and power management frameworks, however
the toolkit is generic enough to accommodate other usages.

The toolkit is based on a set of core libraries:

- [devlib](http://github.com/ARM-software/devlib)
- [TRAPpy](http://github.com/ARM-software/trappy)
- [BART](http://github.com/ARM-software/bart)

TODO: add a figure which represent the toolkit structure

# Installation

This notes assumes an installation from scratch on a freshly installed Debian
system.

## Required dependencies

##### Install build essential tools

	$ sudo apt-get install build-essential autoconf automake libtool

##### Install required python packages

	$ sudo apt-get install python-matplotlib python-numpy

##### Install the Python package manager

	$ sudo apt-get install python-pip python-dev

##### Install required libraries

	$ sudo pip install trappy bart-py devlib

## Clone the repository

TODO: add notes on how to clone and initialize the repostiory



# Toolkit organization

The toolkit provides these resources:

A collection of "assets" required to run examples and tests:
	.
	|-- assets
	|   `-- mp3-short.json

A script to setup the test environment.

	|-- init_env

A collection of IPython Notebooks grouped by topic:

	|-- ipynb
	|   |-- devlib
	|   |-- ipyserver_start
	|   |-- ipyserver_stop
	|   |-- nohup.out
	|   |-- sched_dvfs
	|   |-- utils
	|   `-- wlgen

A set of support libraries which allows to easily develop new IPython notebooks
and tests:

	|-- libs
	|   |-- __init__.py
	|   |-- utils
	|   `-- wlgen

A JSON based configuration for the target device to use for running the tests:

	|-- target.config

A collection of regression tests grouped by topic:

	|-- tests
	|   `-- eas

A collection of binary tools, pre-compiled for different architectures, which
are required to run tests on the target device:

	`-- tools
	    |-- arm64
	    |-- armeabi
	    |-- plots.py
	    |-- report.py
	    |-- scripts
	    `-- x86


# Quickstart tutorial

This section provide a quick start guide on understanding the toolkit by guiding
the user thought a set of example usages.


## 1. IPython server

[IPython](http://ipython.org/notebook.html) notebook is a web based interactive
python programming interface. This toolkit provides a set of IPython notebooks
ready to use. To use one of these notebooks you first need to start a simple
IPython server.

	# Enter the ipynb folder
	$ cd ipynb
	# Start the server
    $ ./ipyserver_start lo

This will start the server and open the index page in a new browser tab.
If the index is not automatically loaded in your browser, visit the link
reported by the server startup script.

The index page is an HTML representation of the local ipynb folder.
From that page we can access all the IPython notebooks provided by the toolkit.


## 2. Setup the TestEnv module

Usually notebooks and tests make use of the TestEnv class to initialize and access
a remote target device. To get an overall view of the functionalities exposed
by this class you can have a look at this notebook:
[utils/testenv_example.ipynb](http://localhost:8888/notebooks/utils/testenv_example.ipynb)


## 3. Typical experiment workflow

RT-App is a configurable synthetic workload generator used by many tests and
notebooks to run experiments on a target. The toolkit provides a python API to
easy the definition of RT-App based workloads and their execution on a target.

This notebook:
[wlgen/simple_rtapp.ipynb](http://localhost:8888/notebooks/wlgen/simple_rtapp.ipynb)
is a complete example of experiment setup, execution and data collection.

Specifically it shows how to:
1. configure a target for an experiment
2. configure FTrace for events collection
3. configure an HWMon based energy meter for energy measurements
4. configure a simple rt-app based workload consisting of two different tasks
5. run the workload on the target to collect FTrace events and energy
   consumption
6. visualize scheduling events using the in-browser trace plotter provided by
   the TRAPpy library
7. visualize some simple performance metrics for the tasks


## 4. Example of a smoke test for sched-DVFS

One of the main aims of this toolkit is to become a repository for
regression tests on scheduler and power management behavior.
A common pattern on defining new test cases is to start from an IPython
notebook to design an experiment and compute metrics of interest.
The notebook can than be converted into a self contained test to run in batch
mode.

An example of such a notebook is:
[sched_dvfs/smoke_test.ipynb](http://localhost:8888/notebooks/sched_dvfs/smoke_test.ipynb)

In this notebook the toolkit API is more extensively used to defined an
experiment which allows to:
1. select and configure three different CPUFreq governor
2. run a couple of RTApp based test workloads in each configuration
3. collect and plot scheduler and CPUFreq events
4. collect and compare the energy consumption during workload execution
   in each of the different configurations

The notebook compare three different CPUFreq governor: "performance", "sched"
and "ondemand". New configurations are easy to add.
For each configuration the notebook generate plots and tabular reports
regarding working frequencies and energy consumption.

This notebook is a good example of usage of the toolkit to define a new set of
experiments which can than be transformed into a standalone regression test.




