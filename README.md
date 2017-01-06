
### Basic usage and ordering

The first script here can be run on golden builds to keep them up to date should ever we need them


1. **provision_servers** : Provisions with common tasks, could be used to bring a bare server up to ceph standard
2. **provision_cluster** : Provisions ceph components osds, mon, rgws and admins
3. **ceph_sanity_check** : Performs sanity checking ceph related attributes to avoid common issues

```
Examples
 ansible-playbook -i inventory/production provision_servers.yml
 use
  --limit : specific hosts
  --tags : specific role

 ansible-playbook -i inventory/golden-builds.yml provision_golden-builds.yml
 use
  --limit : specific hosts
  --tags : specific role
```

###  Details about playbooks

#### Golden-builds:
 Golden builds are kept up to date with any changes that need to be made to the cluster via ansible as well.

* **gld-blds-mon:**
  this can be used for any Ceph service that isn't an osd
* **gld-blds-osd:**
  This is specifically meant for osd create, main difference being it has 2 networks to set up.


### Pre-ceph

#### Provision Servers
 Non cluster breaking, nothing should be added to this that directly effects cluster health. This is meant for
 server prep only.
 
 `'provision_servers.yml'` will run all required tasks on a machine to prepare it for Ceph use
 It does not harm or effect the cluster in anyway. These tasks set up all preflight checks needed for a node
 to be considered 'Ceph ready'. 

 This removes the need for golden build cloning, as the .yml has been tested across golden-builds and the cluste r nodes themselves. All you have to do is add the ip of the new node to the `inventory/production` hosts file and and ansible will change the state of that system. 

 Currently there is a few things the ansible does not prepare that exist in the golden build already:

#######################################
 TODO: http://docs.ceph.com/docs/master/start/quick-start-preflight/
  cephuser creation
  setting sudoless no password login
  dynamic:
    local .ssh/config server access
    a valid /etc/hosts file
  known_hosts file in .ssh/
  visduo require tty
  remove ceph.repo from prx common with conditional

  
 To get around this clone a golden build which has the above set up already. Change the networking and 
 then run this ansible. These tasks will be added an tested against the golden build at a later time. 

