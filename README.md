
## About this fork

This fork introduces the following changes over the original:

- Rewritten test suite allowing for easier addition of new features.
- The init() method which initialized the VM based on the named base box.
- The halt() method which stops the VM without destroying it.
- Support for sandbox mode using the Sahara gem 
  (https://github.com/jedi4ever/sahara).
- Support for box-related commands - box_add(), box_list(), box_remove() methods. 
- Support for provisioning - up() accepts no_provision and there is the provision()
  method.

## Introduction

Python-vagrant is a python module that provdes a thin wrapper around the
`vagrant` command line executable, allowing programmatic control of Vagrant
virtual machines (boxes).  This module is useful for:

- Starting a Vagrant box (`up`).
- Terminating a Vagrant box (`destroy`).
- Querying the status of a box (`status`).
- Getting ssh configuration information useful for SSHing into the box. (`host`, `port`, ...)

This package is _alpha_ and its API is not guaranteed to be stable.  The API
attempts to be congruent with the `vagrant` API terminology, to facilitate
knowledge transfer for users already familiar with Vagrant.

I wanted python bindings for Vagrant so I could programmatically access my
vagrant box using Fabric.  Why are you interested?


## Contribute

If you use python and vagrant and this project does not do what you want,
please open an issue or a pull request on github,
https://github.com/todddeluca/python-vagrant.


## Requirements

- A working installation of Vagrant.
- Vagrant requires VirtualBox.
- Probably python 2.7 (since that is the only version it has been tested with.)


## Installation

### Install from github.com (this fork)

Clone and install python-vagrant

    cd ~
    git clone git@github.com:kamilgrymuza/python-vagrant.git
    cd python-vagrant
    python setup.py install


## Usage

A contrived example of starting a vagrant box (using a Vagrantfile from the
current directory) and running a fabric task on it:

    import vagrant
    from fabric.api import env, execute, task, run

    @task
    def mytask():
        run('echo $USER')


    v = vagrant.Vagrant()
    v.up()
    env.hosts = [v.user_hostname_port()]
    env.key_filename = v.keyfile()
    env.disable_known_hosts = True # useful for when the vagrant box ip changes.
    execute(mytask) # run a fabric task on the vagrant host.
