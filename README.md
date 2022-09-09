# obsit.autovmvalidation.linux

Validation Checks performed on a newly created VM in reference to the SRF used for it's creation. This for FCA environment..

__Checks performed in this role are as follows:__
~~~
- Server hostname is in lower case
- CPUs are allocated as per SRF
- RAM is allocated as per SRF
- EIN IP address set as per the SRF
- Casa IP Check
- FCA Route information updated as per specified environment in SRF for CASA IP.
- Telnet Check for Host:57.203.253.76 on port:80
- DNS Network IPs on server are configured as per SRF
- DNS Search Domain is configured as per SRF
- Verify resolv.conf file entry & check for any spelling mistake
- rscd process is running
- BL agent version should be latest
- InfraVG Size Check as per mentioned in the SRF.
- Display size of ROOTVG allotted to the VM
- Kernel Version Check
- Check if SVC_OPSADMIN Installed is of updated version according to RHEL/Centos version.
- Check if SUDO Package Installed is of updated version according to RHEL/Centos version.
- Check if Shinken Package Installed is of updated version according to RHEL/Centos version.
- Check if docker installed if it is asked to be installed as per SRF
- NTPD Service Status Check
- Check if provided NTP servers are present or not
- VMWare Tools Version should be latest
- FileSystem Details (Mount Point and Space Allocated) mentioned in SRF document is same as those configured on the VM
- Hosts File is having correct Entries for IPAddress and Hostname of the VM
- NSLOOKUP Test to Verify if Hostname resolves to the assigned IPAddress
- User Details ( Existence, UID, GID, Home Directory, Shell assigned and Password Expiry) mentioned in SRF document is same as those configured on the VM
- Directory Details ( Existence, Owner name, Group name and Permissions assigned) mentioned in SRF document is same as those configured on the VM
- Oracle pre-requisite configuration were being performed
- SWAP memory to be validated in case of Oracle Pre-req Configuration was done.
- Oracle user dbadmin existence and parameter check if mentioned in SRF
- Shinken user FS detail visibility check
- User - mlbx Sudo Entry Check if mlbx is mentioned in the SRF
- SSH enabled for all users with their respective passwords on the new VM
- Home Directory Owner Validation for all users mentioned in the SRF
~~~

## Requirements

- Ansible version >= 2.9.3
- Operating systems:
  - Redhat - version: 6, 7, 8 (Santiago, Maipo)
  - Centos - version: 6, 7


- The following ports must be opened:
  - PROTOCOL/PORT and Direction:
    - Port 22 for SSH connectivity

## Role Variables

|Name|Type|Description|Default|Mandatory|
|----|----|-----------|-------|---------|
`ovm_validateBladeLogicServiceVersion`|float-string|Updated Blade Logic Service Version|`8.9.01.68`|Yes
`ovm_validateVMWareToolsVersion`|float-string|Updated VMWare Tools Version Installed on the host|`10.3.23.768`|Yes
`ovm_validateSVC_opsadmin_version`|float-string|Updated SVC_opsadmin Version|`0.1`|Yes
`ovm_validateSVC_opsadmin_release`|float-string|Updated SVC_opsadmin Release|`1`|Yes
`ovm_validateKernelVersion`|float-string|Updated Kernel Version|`3.10.0-957.27.2.el7.x86_64`|Yes
`ovm_validateRhel7Sudo_version`|float-string|Updated SUDO Version for RHEL 7|`1.8.23`|Yes
`ovm_validateRhel7Sudo_release`|float-string|Updated SUDO Release for RHEL 7|`10.el7_9.1`|Yes
`ovm_validateSVC_opsadmin`|string|Updated SVC_opsadmin version for RHEL 8|`svc_opsadmin-0.1-1.noarch`|Yes
`ovm_validateRhel8Sudo_version`|string|Updated SUDO version for RHEL 8|`sudo-1.8.29-6.el8_3.1.x86_64`|Yes
`email`|string|Email Id on which Output file needs to be sent|`<set it as team alias>`|Yes
`varfile`|string|CSV SRF File Name|`<set it as srf_file>`|Yes
`ntp_`|list|List of NTP servers|`<NTP servers>`|Yes

## Role Tags

Tags will be created soon.

## Dependencies

None

## Example Playbook

~~~yaml
---
- hosts: '{{ host_name }}'
  remote_user: osadmin
  ignore_errors: true
  become: true
  roles:
    - obsit.autovmvalidation.linux
~~~

Pre-requisites for Playbook installation
---------------------------------------

- Make sure to keep the XLSX SRF File in /tmp directory of your Ansible Server.
- Add hostname in Ansible Inventory
- Create a work Directory
- Run the parser code with work directory path and SRF_file name as input
   [ Parsing code is available at location - https://gitlab-bypassproxy.casa.uro.equant.com/ocb-csd/pco/obs-it/scripts/parsesrf-yaml-var_vm-val_fca_fe ]


- Once all the pre-reqs are complete, run the ansible code
- Validation will be performed and Result will be emailed.


Steps to run the Parser Code
---------------------------------------

* How to run the parser code:
  Command
   <strong> sudo sh listExcelVars.sh `<Work_Directory_Path>` `<XLSX_SRF_File_Name>` </strong>

Playbook installation
--------------

* Command:

 <strong>  ansible-playbook `<playbook_name>` -i `<Inventory_file_path[Optional]>` --extra-vars "host_name='`<Hostname>`' varfile='`<Parser_Output_Yaml_file>`' email='`<EmailID_of_DL>`'" </strong>

* Launch the playbook as follow:

  ~~~
  $ ansible-playbook <name_and_location_of playbook> -i <inventory-file> --extra-vars "var=values......."

  $ ansible-playbook vm_validate.yml -i /etc/ansible/Inventory/ServerValidation --extra-vars "host_name='VM_NAME or IP' varfile='SRF_parser_output_yaml' email='Email Alias of Team'"

  ~~~


* __Configure the Variables.__

  The list of variables are described above in section "Role Variables".<br />
  You can configure your variables in different locations in the Playbook.<br />
  Suggestion: configure specific variables values for your group of machines by creating a file named as your group of machines (ex: centos7) in the folder "group_vars".<br />
  In this case, the variables configured in folder "group_vars" will overwrite the Global values configured in folder "defaults".

## Copyright 
Copyright (c) 2019, Orange. All rights reserved.

## License
[3-Clause BSD License](LICENSE.md)

## Maintainer/Owner:
* kanika.hans@orange.com
