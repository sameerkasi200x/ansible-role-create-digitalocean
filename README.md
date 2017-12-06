Role Name
=========

This role is to launch a new Digital Ocean Droplet and add to the inventory. You can use it for creating a Droplet with a particular name and then later use another playbook/role to perform tasks on that server e.g. install web server etc.


Requirements
------------
 * Python
 * You need to setup credentials file (and pass the path as a variable cred_file) which has api_key

To do 
--------------

 * Allow users to add an array of volumes.
 * Provide a variable to control whether in the end Ansible inventory and host variables should be updated or not.

Role Variables
--------------
Below is a list of mandatory variables-
 * region: The digital ocean region in which the droplet has to be created. 
 * server: Server name. An entry of this name will be added to host_vars and file indicated by variable {{ inventory_file_path }}. The digital ocean instance will be created with this name and it will be an unique name.
* image_id: Digital Ocean image id which you want to use for this deployment. Default is centos-7-x64. To get list of images that you have access to you can use the digital ocean images API to get the list
```
export API_KEY= your-digital-ocean-key-here
 curl -X GET -H "Content-Type: application/json" -H "Authorization: Bearer ${API_KEY}" "https://api.digitalocean.com/v2/images?page=1&per_page=999&type=distribution&distribution:centos&public:true"
```

 * ssh_key: Id of SSH Key to be added to the instance. Default is . Make sure the ssh key is already added. You can get a list of ssh keys and id with below API call-

```
curl -X GET -H "Content-Type: application/json" -H "Authorization: Bearer ${API_KEY}" "https://api.digitalocean.com/v2/account/keys?page=1&per_page=999"
```

 * instance_size: Size of instance. Defaultis 2gb
 * droplet_user: User to be used for ssh, as per the AMI. Default is root.
 * image_os: Operating System which is setup on the image being used. This will be used for tagging host level variable while adding the node to inventory. Default is CentOS.
 * image_os_version: Operating System version which is setup on the image being used. This will be used for tagging host level variable while adding the node to inventory. Default is 7.
 * inventory_file_path: Path of the inventory file, which will be updated with details of new server. 
 * cred_file: Path of the credentials file (in .ini format) which contains the Digital Ocean credentials. Example file content-

```
[digital_ocean]
api_key=your-digital-ocean-api_key-here

```




Dependencies
------------
This role does not need any other role.

Example Playbook
----------------
```
- name: Create a Digital Ocean Droplet for Primary
  hosts: localhost
  connection: local
  vars:
    server: pg-primary
  roles:
    - create-digitalocean
```

An example for using this role for deploying EDB Postgres is over here in my other GitHub repository [self-provisioned-edb-multiplatform](https://github.com/sameerkasi200x/self-provisioned-edb-multiplatform) in [digital-ocean-server-setup-playbook.yml](https://raw.githubusercontent.com/sameerkasi200x/self-provisioned-edb-multiplatform/master/digital-ocean-server-setup-playbook.yml)


License
-------

GNU

Author Information
------------------
sameer.kasi200x@gmail.com
sameer.kumar@ashnik.com

Twitter: @sameerkasi200x
