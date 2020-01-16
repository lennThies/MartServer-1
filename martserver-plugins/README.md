# MartServer-Plugins
In general two types of MartServer plugins exist that can be generated from OCCI extension created with [OCCI-Studio](https://github.com/occiware/OCCI-Studio). These are model and connector plugins.
Model plugins provide the implementation of the elements modeled within the OCCI extensions. Connector plugins implement the functionality to be performed when service requests are recieved manaing the elements specified in the OCCI extension the connector is generated for. In the test setup used in the [getting started section](../), only dummy connectors were used. For an actual deployment these dummy connectors have to be replaced with actual connectors that forwards incoming requests to the cloud. These can be found in the [live](./live) folder and need to be configured as follows.

## Infrastructure connector
In order to translate the REST requests to requests of the cloud API, the MART server requires a connector for the corresponding cloud. 
Therefore, the connector's jar has to be placed in the martserver-plugins folder before the server is started. 
Currently, only a prototypical [connector for OpenStack](https://github.com/occiware/MoDMaCAO) is provided as part of the MoDMaCAO framework.
However, also this connector has to be configured.
The settings itself can be found in the openstack.properties file contained in the org.modmacao.openstack.connector.jar.  An example is shown in the following Listing:
```
openstack_username = username
openstack_tenant = testTenant
openstack_password = password
openstack_endpoint = http://192.168.34.1:5000/v2.0
#Managmenet network id
openstack_default_network = 75a4639e-9ce7-4058-b859-8a711b0e2e7b
openstack_default_image = adf63ddc-debe-4d7e-b899-b936e989439f
openstack_default_flavor = 36637a26-fded-4635-b6c5-ec8ec0745eab
```

We provided an example properties file next to the connector plugin.
You can edit it and place it into the jar by using the following command:

```
zip -u org.modmacao.openstack.connector.jar ./openstack.properties
```
*Note:* This connector currently only supports version 2 of the openstack authentication service.
*Note:* In our scenarios we have chosen the management network as default network and an Ubuntu 18.04 with Python installed as default image.

## Configuration management connector
To use the configuration management tools of MoDMaCAO some adjustments have to be performed.
These adjustments configure the path to the ansible roles located on the MART server, as well as the location of the playbook.
Moreover, the ansible user and the location of the private key to be used to connect to spawned virtual machines have to be configured.

The settings itself can be found at martserver-plugins/org.modmacao.cm.jar in the ansible.properties file.
An example of this configuraiton is shown in the following Listing:
```
ansible_rolespath = ../cm/ansible/roles/
ansible_bin = /usr/bin/ansible-playbook
ssh_timeout = 100
ansible_user = ubuntu
private_key_path = /home/ubuntu/key.pem
```

We provided an example properties file next to the connector plugin.
You can edit it and place it into the jar by using the following command:

```
zip -u org.modmacao.cm.*.jar ./ansible.properties
```


## Additional plugins
Amongst others the following [plugins](/martserver-plugins) are used within the provided setup of the MartServer:
- [WOCCI](https://gitlab.gwdg.de/rwm/de.ugoe.cs.rwm.wocci)
    - A workflow extension for OCCI.
- [MOCCI](https://gitlab.gwdg.de/rwm/de.ugoe.cs.rwm.wocci)
    - A monitoring extension for OCCI.
- [OCCI](https://github.com/occiware/OCCI-Studio) Core, Crtp, Infrastructure
 - Basic extensions to create OCCI base resources.
    - In this setup a connector to a provide [Openstack](https://www.openstack.org/) Cloud ([Newton](https://releases.openstack.org/newton/)) is provided.
- [MoDMaCAO](https://github.com/occiware/MoDMaCAO) Core, CM, Placement, Ansible
    - A set of extensions to couple configuration management systems with infrastructure resources.
    - Currently, Ansible and Saltstack are supported.