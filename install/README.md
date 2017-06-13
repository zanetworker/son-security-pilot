# Virtual Personal Security Applliance (vPSA) - Create, Manage, Upgrade and Destroy (CMUD)

'son-cmud' provides a simple way to Create, Manage, Upgrade and Destroy (CMUD) vPSA resources like:
* automatic built of a vPSA appliance based on individual VM's for each component - eg: create standalone VM and install Squid, Snort, pfSense from the scratch
* instantiate dedicated VM's based on pre-built images containing the service - eg: squid-3.5.12-u16.qcow2
* deploy all or individual vPSA components to the local VM
* automatic build of a vPSA appliance running individual Docker images of each component (aka, dockerized vPSA version)

All you need is a 'bash' shell with Ansible installed: actually, 'son-cmud.yml' can be used to invoke all the vPSA CMUD operations.


##  Characteristics

* Multi-PoP: deploy to multiple sites (set the right credential variables)
* Multi-VIM: Openstack (AWS under evaluation, OSM under evaluation)
* Multi-Distro: Ubuntu 14.04, Ubuntu 16.04, CentOS 7
* Multi-Operations: Create, Manage, Upgrade, Destroy


## Requirements

* requires Ansible 2.3.0
* requires Shade 1.16.0+


#### Pre-configuration

1. Create the hidden file that contains the available Openstack clouds you can connect [os_client_config](http://docs.openstack.org/developer/os-client-config/)
* ~/.config/openstack/clouds.yaml

2. Select the platment you want to deploy in 'ansible.cfg' (default: "inventory = group_vars/'PLAT'"):<br>
* inventory = "inventory/'PLAT'

3. To avoid setting password credentials, use the private key pair (eg, "~/.ssh/YOUR-KEY.pem") of the public key you have used to create the VM


## Usage

* git clone https://github.com/sonata-nfv/son-security-pilot.git
* cd son-security-pilot/cmud
* ansible-playbook son-cmud.yml -e "ops=[CREATE/MANAGE/UPGRADE/DESTROY] plat=[VPSA|SQUID|SNORT|PFSENSE] pop=[NCSRD|ALABS] distro=[trusty|xenial|Core]"


### Create a VM and install Squid

* ansible-playbook son-cmud.yml -e "ops=create plat=squid pop=alabs proj=demo distro=Core"

### Create a VM and install Snort

* ansible-playbook son-cmud.yml -e "ops=create plat=snort pop=ncsrd proj=qual distro=xenial"

### Create a VM based on an existing image (vCacheContent provides Squid services)

* ansible-playbook son-cmud.yml -e "ops=create plat=vcc  pop=alabs proj=demo distro=Core"

### Deploy Squid to the local VM

* ansible-playbook utils/deploy/squid.yml

### Create a VM and deploy a dockerized version o vPSA (includes squid, snort, pfsense)

* ansible-playbook son-cmud.yml -e "ops=create plat=docker-vpsa pop=alabs proj=demo distro=Core"


### Dependencies

To deploy infrastrucutre resources to an Openstack VIM, the [Openstack command line clients](http://docs.openstack.org/user-guide/common/cli-install-openstack-command-line-clients.html) must be locally installed (already included in the 'son-cmud.yml' playbook)


## Lead Developers

The following developers are responsible for this repository and have admin rights.

* Alberto Rocha (arocha7)


## Contributing

To contribute to the development of vPSA you have to fork the repository, commit new code and create pull requests.


## License

'son-security_pilot' is published under Apache 2.0 license. Please see the LICENSE file for more details.