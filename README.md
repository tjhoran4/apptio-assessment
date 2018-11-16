# ansible webapp deployer

[![Build Status](https://travis-ci.com/tjhoran4/apptio-assessment.svg?branch=master)](https://travis-ci.com/tjhoran4/apptio-assessment)

This is a small ansible role created to deploy a static web application by any user to a test system. This was tested on ubuntu destinations but should work on other linux. The webapp artifact is created upstream and stored here:

  - https://s3-us-west-2.amazonaws.com/techops-interview-webapp/webapp.tar.gz

### Installation

The only pre-requisite for this role, is that ansible is installed on the developer's system in order to perform remote deployments.  For local deployments, ansible can be installed on the remote system only.

To verify that ansible is installed, you can perform the following:

```
$ ansible --version
```
Output should look similar to the following:
```
ansible 2.5.1
  config file = /etc/ansible/ansible.cfg
  configured module search path = [u'/home/tj/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python2.7/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 2.7.15rc1 (default, Nov 12 2018, 14:31:15) [GCC 7.3.0]
```
To install the role itself, you need only to check this out from the git repository into your local directory:

```sh
$ git clone https://github.com/tjhoran4/apptio-assessment.git
```
> Alternatively, you can download the archive using curl/wget at: https://github.com/tjhoran4/apptio-assessment/archive/master.zip

### Usage
This deployment script has no required configuration variables in order to run.  The destination hosts (or localhost), will be passed as part of the command.  This will check if someone has already run this webapp on the same machine and if so, not deploy your copy.

> note that all commands are run from the base of the deployed directory by default.  i.e. apptio-assessment/ if checked out.  Further config is necessary to install role globally if needed.

**local single system install**
```
$ ansible-playbook -i localhost, -c local webapp.yaml
```
**remote single system install**
```
$ ansible-playbook -k -i localhost, webapp.yaml
```
**remote multi system install**
```
$ ansible-playbook -k -i host1,host2,host3, webapp.yaml
```
> note the comma after localhost in these cases, this denotes an inline inventory as opposed to using an actual inventory file.  For further information on inventory, if you would like do perform more advanced actions, see: https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html

**Alternate install path**
If the default path of ~/webapp is not desired, you can pass a webapp_dest_dir path to the command with the -e flag:
```
ansible-playbook -i localhost, -c local -e "webapp_dest_dir=<dest path>" webapp.yaml
```
### Potential issues:
Strict host key checking:
> See https://docs.ansible.com/ansible/2.5/user_guide/intro_getting_started.html#id7

Proxy issues for non internet connected hosts:
Ansible will use proxies if the system has the proper linux environment variables set.  If not these need to be set by the user.
> See: https://docs.ansible.com/ansible/latest/modules/get_url_module.html

