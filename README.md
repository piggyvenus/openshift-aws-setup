# openshift-aws-setup

## Overview

This repo is a set of Ansible automation scripts that:
 - provisions AWS infrastructure
 - deploys Openshift (Origin or Container Platform)

AWS related configuration can be customised by modifying ```vars/aws-config.yaml```. Note that the number of application nodes is configurable, the default is 3.

The OpenShift inventory can be customized by modifying ```roles/openshift-install/files/openshift_inventory.cfg```, note that dynamic metrics and logging is enabled by default as well as the AWS cloud provider.

## Prerequisites

 - AWS credentials: access key & secret --> http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html
 - Ansible installed --> http://docs.ansible.com/ansible/intro_installation.html
 - Route 53 Public Hosted Zone. The playbook use Route53 for private and public DNS and routes. While the playbook automatically creates everything for the private DNS hosted zone and routes, it is expected that you have a public hosted zone in Route 53 that matches whatever you are using for the public DNS.
 - By default a single admin user is created called ```admin``` that can be used to login into the web console. The password for the user is in the ```aws-config.yml``` vars file. __PLEASE CHANGE THE PASSWORD TO SOMETHING UNIQUE!__. Other users can be added to this file as well.

## Run

You need to export your AWS credentials prior to running:

```
export AWS_ACCESS_KEY_ID=<AWS_ACCESS_KEY_ID>
export AWS_SECRET_ACCESS_KEY=<AWS_SECRET_ACCESS_KEY>
```

For OpenShift Origin, the command ```./openshift-playbook-run.sh``` will execute the Ansible playbook
with a set of roles which will provision AWS infrastructure and install Openshift on top of that.

Installing OpenShift Container Platform (OCP) requires a Red Hat subscription and you must provide your Red Hat credentials
and the name of the pool to use to the script.

```
./openshift-playbook-run.sh -e rhsm_username=me@something.com -e rhsm_password=mypassword -e rhsm_pool="sas876sa8sa76sjk..."
```
Note the above is just an example, please update all variables including the pool name which is correct for your situation.

## Network topology

![Network Diagram](./docs/network-topology-openshift.jpg)

A private VPC and DNS is used, OpenShift is installed using the private IP addresses. This means the IP addresses never change, unlike EC2 public addresses, and the environment can be stopped and started as needed.

## References

 - https://www.codeproject.com/Articles/1168687/Get-up-and-running-with-OpenShift-on-AWS
 - https://docs.openshift.org/latest/welcome/index.html
 
