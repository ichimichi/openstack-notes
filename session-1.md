### Controller Node

Runs all openstack services

* Horizon - (Dashboard) gives the UI
* Nova - (Compute Service) used for managing the resources in the virualized environment
* Neutron - reponsible for networking and it manages ip addresses, router, switches, firewall
* Keystone - (Identity Service) repository of all users and their permissions for the services which they are using
* Glance - (Image Service) provide the images to openstack - e.g, cirros, fedora, ubuntu, etc. 
* Cinder - (Block Storage) virtual storage service for the instances
* Swift - (Object Storage) store and retrieve data in cloud. Unstructured Data.
* Heat - (Orchestration Purpose) allow developers to store the cloud application as a file
* Manila - (Shared File System) e.g, iSCSI in linux

