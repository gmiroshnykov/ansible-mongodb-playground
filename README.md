Ansible MongoDB Playground
==========================

This repository contains an example setup of three MongoDB instances
running on two VMs:

* `master.mongodb.dev` has a primary instance running on default port
and arbiter instance running on port 30000
* `slave.mongodb.dev` has a secondary instance running on default port

Requirements
------------

* [Vagrant 1.4+](http://www.vagrantup.com/downloads.html) (tested on 1.4.2)
* A forked version of Ansible (`pip install https://github.com/laggyluke/ansible/archive/mongodb_improvements.zip`) which contains the following changes:
    * ansible/ansible#5597
    * ansible/ansible#5615

Usage
-----

1. Add the following lines to your `/etc/hosts`:

    ```
    10.0.40.2 master.mongodb.dev
    10.0.40.3 slave.mongodb.dev
    ```

2. Run the following commands:

    ```
    vagrant up
    vagrant ssh master
    mongo
    rs.status()
    ```

3. The output of `rs.status()` should look like this:

    ```javascript
    {
        "set" : "rs0",
        "date" : ISODate("2014-01-14T13:01:24Z"),
        "myState" : 1,
        "members" : [
            {
                "_id" : 0,
                "name" : "master.mongodb.dev:27017",
                "health" : 1,
                "state" : 1,
                "stateStr" : "PRIMARY",
                "uptime" : 196,
                "optime" : Timestamp(1389704374, 1),
                "optimeDate" : ISODate("2014-01-14T12:59:34Z"),
                "self" : true
            },
            {
                "_id" : 1,
                "name" : "slave.mongodb.dev:27017",
                "health" : 1,
                "state" : 2,
                "stateStr" : "SECONDARY",
                "uptime" : 133,
                "optime" : Timestamp(1389704374, 1),
                "optimeDate" : ISODate("2014-01-14T12:59:34Z"),
                "lastHeartbeat" : ISODate("2014-01-14T13:01:23Z"),
                "lastHeartbeatRecv" : ISODate("2014-01-14T13:01:23Z"),
                "pingMs" : 1,
                "syncingTo" : "master.mongodb.dev:27017"
            },
            {
                "_id" : 2,
                "name" : "master.mongodb.dev:30000",
                "health" : 1,
                "state" : 7,
                "stateStr" : "ARBITER",
                "uptime" : 110,
                "lastHeartbeat" : ISODate("2014-01-14T13:01:24Z"),
                "lastHeartbeatRecv" : ISODate("2014-01-14T13:01:23Z"),
                "pingMs" : 0
            }
        ],
        "ok" : 1
    }
    ```