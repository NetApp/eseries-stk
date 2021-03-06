## E-Series Solution ToolKit (ESTK)

This repository contains ansible playbooks that assist in configuring NetApp E-Series and attached hosts for use with a particular application.

#### Table of Contents

1. [Disclaimer](#disclaimer)
2. [Overview](#overview)
3. [Requirements](#requirements)
4. [To Docker or Not to Docker](#todockerornottodocker)
5. [TODOs and Future plans](#TODOsandFutureplans)

### Disclaimer

These Ansible playbooks are written as best effort and provide no warranties or SLAs, expressed or implied.

Tested control server distros

1. Debian 8 with Docker on Windows 10
2. CentOS 7.3
  
Tested host distributions

1. CentOS 7.3
2. RHEL 7.4
3. SUSE 12.3
4. Ubuntu 16.04
  
Tested Santricity versions

1. 08.40  
  
### Overview

The Ansible playbooks contained within are modular in nature and depending on what is defined in your hosts file, a small 
portion of the overall playbooks will be used.

### Requirements

It is recommended to have SSH keys setup prior to installation. 
Package installers (apt, yum, etc...) need to be configured.  

### Configuration
#### To use the docker workflow:

1. Run the docker file with the following commands:
   * docker build https://raw.githubusercontent.com/NetApp/eseries-stk/master/docker/ansibleDev/Dockerfile -t eseries-stk
   * docker run --rm -it eseries-stk

2. Update git repository to ensure you are on the latest commit 
   * cd /home/eseries-stk
   * git pull

3. Fill out /home/eseries-stk/hosts file with a single entry per line.
   * splunk_eseries:  Any array that should be provisioned for splunk.
      * For E-Series arrays, just use the ip address for the first controller.
   * fc_hosts:  Host servers that are connected to the array via the fibre channel host ports
   * sas_hosts:  Host servers that are connected to the array via the SAS host ports
   * splunk_indexers:  Index servers that splunk is being deployed on.  
      * This list should be the same as the protocol hosts such as fc_hosts or sas_hosts.

4. Fill out /home/eseries-stk/group_vars/all file.
   * For E-Series arrays, set your user/pass for E-Series (api_username, api_password) in /home/eseries-stk/group_vars/all file.

5. Run the Ansible playbook with the following command from /home/eseries-stk directory.
   * ansible-playbook -i hosts splunk.yml

#### Non-docker workflow:
 
1. Requires Unix host with network connection to all hosts and E-Series storage arrays. 

2. Verify that you have the following packages installed:
    *  Python 2.7.6
	*  Ansible 2.3.2.0
	*  Git

3. Clone the git repository:
    * cd /home/eseries-stk
    * git clone https://github.com/NetApp/eseries-stk

4. If you are using a directory other than /home/eseries-stk you will need to update library variable in ansible.cfg to include the path to the roles/eseries/library directory.

5. Continue with step three from docker workflow above.
 
### TODOs and Future plans

1. Find a better way to pass in array IP addreses and passwords other than group_vars/all

2. Use smaller docker base image. 

### Support

For questions or queries, please email `ng-eseries-solutiontoolkit@netapp.com`. 

Please enter an issue if you would like to report a defect.
