# NFS sandbox

This repository contains infrastructure to setup a client/server to reproduce
NFS related issues easily.

Please note that this setup assumes that you have some SLES12SP1 repositories
under `http://192.168.0.200/repos/`. Please edit the Vagrantfile as per your
needs.

To start the infrastructure, type:

    vagrant up

Once the machines are up, you can ssh into them with:

    vagrant ssh client
    vagrant ssh server

## Useful Things

If you want to emulate blocked I/O, on the client, say:

    iptables -A OUTPUT -d 192.168.111.11 -j DROP

To remove the blockage, say:

    iptables -D OUTPUT -d 192.168.111.11 -j DROP
