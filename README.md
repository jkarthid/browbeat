# Browbeat
Scripts to help determine number of workers a given OpenStack service needs.

# Before running browbeat
* Execute the ansible/gen_hostfile.sh script (builds the ssh config)
* Install Tools (rally , shaker, connmon, etc)
* Configure browbeat-config to match your tests

# How to run Browbeat?
On the Red Hat OpenStack Director host, as the Stack user jump into a venv w/ Rally and you simply run:

    ./browbeat.sh <test name>

# What is necessary?
* Red Hat OpenStack Director
  * Why? We use the keyless ssh to reach each controller instance and compute instance.
* OpenStack Rally
  * Why? We are using Rally to stress the control plane of the env.
* Ansible
  * Why? We started with using bash to make changes to the Overcloud, creating complex sed/awks that we get for free with Ansible (for the most part). If you prefer to not use Ansible, the older versions (no longer maintained) of the browbeat.sh can be found here.


# Example Install and Run

On director (As stack user):
```
$ git clone https://github.com/jtaleric/browbeat.git
$ cd browbeat/ansible
$ ./gen_hostfile.sh localhost ~/.ssh/config
$ cd ~
$ sudo yum install -y libffi-devel gmp-devel postgresql-devel
$ wget -q -O- https://raw.githubusercontent.com/openstack/rally/master/install_rally.sh | bash
$ . ~/rally/bin/activate
$ . ~stack/overcloudrc
$ rally deployment create --fromenv --name overcloud
$ wget http://cloud.centos.org/centos/7/images/CentOS-7-x86_64-GenericCloud.qcow2
$ glance image-create --name centos7 --disk-format=qcow2 --container-format=bare < CentOS-7-x86_64-GenericCloud.qcow2
$ glance image-update centos7 --is-public true
$ . ~stack/stackrc
$ cd browbeat
$ sudo pip install -Ur requirements.txt
$ ./browbeat.sh test01
```
Edit `browbeat-config` for desired tests before running `browbeat.sh`
