# S3F/S3FNet

[SSF (Scalable Simulation Framework)](http://www.ssfnet.org/) defines an API that supports modular construction of simulation models, with automated exploitation of parallelism. Building on ten years of experience, we have developed a second generation API, named “S3F”, to better reflect use and support maintainability.

We have also developed a network simulation/emulation testbed, named “S3FNet”, on top of S3F, for large-scale system analysis and evaluation. Figure System Architecture shows the overall system architecture with following three layers:

* S3F: the kernel of the system
* S3FNet: a parallel network simulator

S3F defines an API for parallel discrete-event simulation, and has the following major improvements:

* S3F uses only standard language and library features found in the GNU C++ compiler, g++.
* Time advancement is broken up into epochs, between which control is released from the simulation threads to allow other activity (e.g., recalculation of received radio strength maps and location of mobile devices).
* S3F gives a modeler extreme control over the ordering of process body executions.

S3FNet is a network simulator built on top of the S3F kernel. It is capable for creating communication network models with network devices (e.g., host, switch, and router) and layered protocols (e.g., TCP/IP, OpenFlow). 
The main features of S3FNet are summarized as follows:

* Parallel discrete-event simulation
* Distributed emulation over TCP/IP networking
* Application lookahead (neural-network-based model)

# Installation

## Download

```shell
% git clone https://github.com/JinLabIIT/S3FNet.git
```



## Software Requirement

Software packages required for building different features of S3F/S3FNet are summarized below. The apt-get commands for Debian-based platforms are just used as examples.

- S3F/S3FNet base version

  No special packages required.

- OpenFlow Simulation

  Required package: libxml, automake, flex, boost, Python 2.7.5

  To install libxml, automake, flex:

```shell
% sudo apt-get install libxml2-dev
% sudo apt-get install automake
% sudo apt-get install flex
```

​		To install boost library:

```shell
% sudo apt-get install libboost-all-dev
```

​		To install Python-2.7.5:

```shell
% wget http://www.python.org/ftp/python/2.7.5/Python-2.7.5.tgz
% tar xfvz Python-2.7.5.tgz
% cd Python-2.7.5
% ./configure
% make
% make install
```

## Build

The build.sh script in S3F’s root directory is used for building the S3F/S3FNet project:

```shell
% ./build.sh
```

Usage: build.sh [-h] [-n arg] [-f] [-i] [-s] [-l] [-d]

| -n   | number of cores used to run make                             |
| ---- | ------------------------------------------------------------ |
| -f   | skip building those libraries that only need to be built once (DML, metis, openflowlib) |
| -i   | incremental build (update) instead of clean build            |
| -d   | display S3FNet debug messages                                |
| -h   | display the usage                                            |
| -s   | simulation only, not building emulation [Full Version Only]  |
| -l   | enable emulation lookahead [Full Version Only]               |

*Examples:*

Use 4 cores to clean build the S3F project for the first time:

```shell
% build.sh -n 4
```

Use 4 cores to clean build the S3F project excluding those libraries that only need to be built once:

```shell
% build.sh -n 4 -f
```



# Running S3F/S3FNet Experiments

Once the project has been built successfully, it is time to run some simulation experiments. Here, we are going to run two experiments:

1. PHOLD model (a simple S3F-based application)
2. A server-client network model downloading files through UDP protocol (a test case in S3FNet)

How to develop your own network protocol and application is described in [*Develop your own protocol/application*](https://s3f.iti.illinois.edu/usrman/s3fnet.html#s3fnet-dev), and the logical organization of S3F simulation program is presented in [*Logical Organization of S3F Simulation Program*](https://s3f.iti.illinois.edu/usrman/s3f.html#s3f-logic-org).

To run the PHOLD model:

```shell
% cd app
% ./phold_node 4 2 10
```

The results may look like:

```shell
enter window 1
completed window, advanced time to 10
enter window 2
completed window, advanced time to 20
enter window 3
completed window, advanced time to 30
enter window 4
completed window, advanced time to 40
enter window 5
completed window, advanced time to 50
enter window 6
completed window, advanced time to 60
enter window 7
completed window, advanced time to 70
enter window 8
completed window, advanced time to 80
enter window 9
completed window, advanced time to 90
enter window 10
completed window, advanced time to 100
-------------- runtime measurements ----------------
Simulation run of 0.0001 sim seconds, with 2 timelines
       configured without smart pointer events
       configured without smart pointer activations
total run time is 0.004684 seconds
simulation run time is 0.004392 seconds
accumulated usr time is 0.004 seconds
accumulated sys time is 0 seconds
total evts 59, work evts 34, sync evts 0
total exec evt rate 13433.5, work evt rate 7741.35
----------------------------------------------------
```

To run the UDP server-client model:

```shell
% cd s3fnet/test/udp_oneflow_2timeline
% make
  ../../dmlenv -b 10.10.0.0 oneflow.dml > oneflow-env.dml
  ../../dmlenv -r all oneflow.dml > oneflow-rt.dml
% make test
```

The results may look like:

```shell
../../s3fnet *.dml
Input DML files:
oneflow-env.dml
oneflow-rt.dml
oneflow.dml

enter epoch window 1
2,639,721: UDP client "10.10.0.2" downloaded 100000 bytes from server "10.10.0.18", throughput 487.887878 Kb/s.
14,279,442: UDP client "10.10.0.2" downloaded 100000 bytes from server "10.10.0.18", throughput 487.887878 Kb/s.
25,919,163: UDP client "10.10.0.2" downloaded 100000 bytes from server "10.10.0.18", throughput 487.887878 Kb/s.
37,558,884: UDP client "10.10.0.2" downloaded 100000 bytes from server "10.10.0.18", throughput 487.887878 Kb/s.
49,198,605: UDP client "10.10.0.2" downloaded 100000 bytes from server "10.10.0.18", throughput 487.887878 Kb/s.
60,838,326: UDP client "10.10.0.2" downloaded 100000 bytes from server "10.10.0.18", throughput 487.887878 Kb/s.
72,478,047: UDP client "10.10.0.2" downloaded 100000 bytes from server "10.10.0.18", throughput 487.887878 Kb/s.
84,117,768: UDP client "10.10.0.2" downloaded 100000 bytes from server "10.10.0.18", throughput 487.887878 Kb/s.
95,757,489: UDP client "10.10.0.2" downloaded 100000 bytes from server "10.10.0.18", throughput 487.887878 Kb/s.
completed epoch window, advanced time to 100000000
Finished
-------------- runtime measurements ----------------
Simulation run of 100 sim seconds, with 2 timelines
     configured without smart pointer events
     configured without smart pointer activations
total run time is 0.319977 seconds
simulation run time is 0.318845 seconds
accumulated usr time is 0.0260015 seconds
accumulated sys time is 0.356022 seconds
total evts 6364, work evts 3637, sync evts 0
total exec evt rate 19959.5, work evt rate 11406.8
----------------------------------------------------
```

# Documentation

For more documentation, you can access [S3F website](https://s3f.iti.illinois.edu/). 

We are migrating documents from website to Github. 

# File Organization

The S3F project structure is summarized as follows:

```
├── api                                # S3F core API: Entity, InChannel, OutChannel, Timeline, Event ...
├── app                                # S3F application: phold_node
├── aux                                # synchronization barriers
├── build.sh                           # script for building the S3F/S3FNet project
├── dml                                # dml library
├── doc                                # documentation: user manual, doxygen documents ...
├── metis                              # metis library
├── rng                                # random number generator
├── s3f.h                              # Header file of the S3F project
├── s3fnet                             # S3FNet network simulator
└── time                               # event list
```
