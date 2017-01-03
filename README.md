# Ansible DHCP Role
-------

This is an Ansible role that will provision a fresh DHCPD server installation. DHCP stands for Dynmaic Host Configuration Protocol, and is used to hand out IP addresses from a pre-selected range on your LAN. It allows other hosts the ability to obtain an IP address that will allow the host to communicate with other hosts that reside on the same Local Area Network.

<br>

## More Documentation
-------
[clusterfrak.com](http://clusterfrak.com/devops/ansible/ansible_dhcpd/)

<br>

## Requirements
-------

__1. &nbsp;&nbsp; Install dependencies:__ <br>

> RedHat based distros (RHEL, CentOS):

```bash
sudo yum -y install epel-release
sudo yum clean all
sudo yum -y install ansible
```

<br>

__3. &nbsp;&nbsp; Create directory structure:__ <br>

Create the directory structure that you are going to use. In this tutorial we are going to set up ansible roles in __/etc/ansible/roles__

<br>

```bash
mkdir -p /etc/ansible/roles || exit 0
```
<br>

__4. &nbsp;&nbsp; Set ansible host:__

Set Ansible localhost entry so that ansible knows it will run against localhost and can talk to itself on localhost without attempting to open a TCP socket connection. 

<br>

```bash
echo localhost ansible_connection=local > /etc/ansible/hosts
```

<br>

## Role Variables
-------

The clusterfrak.dhcpd role uses a few environment variables to automatically configure dhcpd. The role is set with default values for each of the available variables. Ansible will attempt to gather shell environment variable values and use those values to over-ride the default values that are set. If no shell environment variable is available or set, then ansible will configure itself to use the default values. In order to customize the installation of dhcpd, simply export the ansible corresponding shell variable to set the value to something other than default prior to installing the role.

<br>

> Ansible Variables:

 - domain: mydomain.local
 - domain_ns: ns1.mydomain.local
 - range_start: 192.168.0.32
 - range_end: 192.168.0.63

<br>

> Mapped Shell Environment Variables:

 - DOMAIN - FQDN that DHCP will be configured to provide IP address services for. This MUST be an FQDN [default:mydomain.local]
 - NAME_SERVERS - FQDN or IP addresses of Name Servers on your LAN. DHCP will forward this out to requesting hosts, configuring those hosts to use the same nameserver for local resolution.
 - DHCP_START_IP - The first IP address that DHCP is allowed to hand out to requesting hosts.
 - DHCP_END_IP - The last IP address that DHCP is allowed to hand out to requesting hosts.

<br>

 > Setting Shell Environment Variables:

 To set a variable value simply export the variable prior to running the role install playbook.

<br>

```bash
export DOMAIN="mydomain.com"
export NAME_SERVERS="ns1.mydomain.com"
export DHCP_START_IP="10.0.0.64"
export DHCP_END_IP="10.0.0.127"
```

 <br>

## Dependencies
-------

There are no required pre-requisites in order to install and run this role.

<br>

## Example Playbook With Default Values
-------

This playbook will set up dhcpd, DHCP will be automatically configured to use the servers IP address, to calculate the subnet, subnet mask, and gateway of your LAN. It will set a DHCP scope that will use the mydomain.local domain. It will set the name server attribute to ns1.mydomain.local, and set the DHCP range from 192.168.0.32 to 192.168.0.63. 

    - hosts: dhcp-servers
      become: true
     roles:
       - clusterfrak.dhcpd

## Example Playbook With Custom Values
-------

This playbook will set up dhcpd, DHCP will be automatically configured to use the servers IP address, to calculate the subnet, subnet mask, and gateway of your LAN. It will set a DHCP scope that will use the customdomain.com  domain. It will set the name server attribute to ns1.customdomain.com, and set the DHCP range from 10.0.0.64 to 10.0.0.127.

'''bash
export DOMAIN="customdomain.com"
export NAME_SERVERS="ns1.customdomain.com"
export DHCP_START_IP="10.0.0.64"
export DHCP_END_IP="10.0.0.127"
'''

    - hosts: dhcp-servers
      become: true
      roles:
        - clusterfrak.dhcpd

## License
-------

BSD

## Author Information
-------

[Rich Nason](http://nason.co) <br>
[Clusterfrak Doc Site](http://clusterfrak.com) <br>
[Container Doc Site](http://appcontainers.com) <br>

