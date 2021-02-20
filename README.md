spread-certificate
==================

This role copies an elasticsearch certificate file across a multi instance cluster. It assumes the certificate is already generated and sits in the configuration directory of a given node in a given cluster.
It assumes your node names in the cluster are in the form of `<hostname>-<base_instance_name><instance_number>`, for an example: `elastic-server1-main1`, and the the instances config directories are named `<base_instance_name><instance_number>`, which implies in out example: `main1`.
The generator host should be described in the inventory file in the group `generator`, and the generator node should be described in the role variables.

Requirements
----------

A generated certificate file that sits in the configuration directory of a certain node.

Role Variables
--------------

variable name | type | description | default value
------------- | -------- | ------------------------ | -------------
base_directory | string | the directory that contains all the instances config directories | /etc/elasticsearch
es_base_instance_name | string | base instance name as decribed in the role general description | main
es_generator_instance_name | string | name of the elastic instance that generated the certificate, not including server name and hyphen | main1
certificate_file_name | string | name of the certificate file, including file extention | elastic-certificates.p12
override_existing_certificate | boolean | if you set this to true, existing certificate files in the instances config directories will be overriden | false


Dependencies
------------

No dependency, although the role uses rsync, but there's a task that install it.

Example Playbook and Inventory
------------------------------
Playbook:

    - name: spread certificate
      hosts: hosts
      roles:
        - role: spread-certificate
      vars:
        es_base_instance_name: instance
        es_generator_instance_name: instance2
        override_existing_certificate: true

Inventory:
> :exclamation: **You must specify the generator group** containing one host which is the host that contains the node that generated the certificate.

    [generator]
    elasticserver1
    [hosts]
    elasticserver1
    elasticserver2
    elasticserver3

License
-------

BSD

Author Information
------------------

Just a frustrated user who didn't want to do this manually.
