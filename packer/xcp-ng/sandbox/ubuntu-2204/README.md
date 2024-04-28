https://xcp-ng.org/blog/2024/02/22/using-packer-with-xcp-ng/

run `packer init` in order to download the plugin if you have an internet connection. If there's a proxy in the way then set:... otherwise download the plugin in install in ~/.packer.d.... Running this script/playbook will setup you environment with the options you've chosen & optionally create the template

## create a quick hosts list to check the templates 

`echo -e -n "all:\n   hosts:\n";for i in $(seq 1 5); do echo "3l-hl-xe0${i}" >> host`

# This will prompt a password and do a quick connection ping

ansible -m ping -i hosts.yml all -u root -k
# -k asks prompts for ssh pass. 
# this lil bit will run `xe template-list` on each of the xcp-ng(or xenserver, untested by me, but iirc they said it's a compatible api or something) and I'm  just checking for overlapping template names. 

ansible -m shell -a 'xe template-list | grep -i ubuntu'   -i hosts.yml all -u root 
-k

# In my case I have a xoa & self build xo. A couple of the hosts have gotten part of the quarterly pathing rollout, but the core infra hosts havent... 3 of them have... welll

```
SSH password: 
[DEPRECATION WARNING]: Distribution centos 8.2.1 on host 3l-hl-xe03 should use /usr/libexec/platform-python, but is using /usr/bin/python for backward compatibility with prior 
Ansible releases. A future Ansible release will default to using the discovered platform python for this host. See 
https://docs.ansible.com/ansible/2.10/reference_appendices/interpreter_discovery.html for more information. This feature will be removed in version 2.12. Deprecation warnings 
can be disabled by setting deprecation_warnings=False in ansible.cfg.
3l-hl-xe03 | CHANGED | rc=0 >>
          name-label ( RW): Ubuntu Focal Fossa 20.04
          name-label ( RW): 3l-xe03 Ubuntu 20.04 Cloud-Init 71
          name-label ( RW): Ubuntu Bionic Beaver 18.04
          name-label ( RW): Ubuntu Xenial Xerus 16.04
          name-label ( RW): Ubuntu Jammy Jellyfish 22.04
[DEPRECATION WARNING]: Distribution centos 8.2.1 on host 3l-hl-xe05 should use /usr/libexec/platform-python, but is using /usr/bin/python for backward compatibility with prior 
Ansible releases. A future Ansible release will default to using the discovered platform python for this host. See 
https://docs.ansible.com/ansible/2.10/reference_appendices/interpreter_discovery.html for more information. This feature will be removed in version 2.12. Deprecation warnings 
can be disabled by setting deprecation_warnings=False in ansible.cfg.
3l-hl-xe05 | CHANGED | rc=0 >>
          name-label ( RW): Ubuntu Jammy Jellyfish 22.04
          name-label ( RW): Ubuntu Bionic Beaver 18.04
          name-label ( RW): Ubuntu Focal Fossa 20.04
          name-label ( RW): Ubuntu Xenial Xerus 16.04
          name-label ( RW): Ubuntu 20.04 Cloud-Init 71
[DEPRECATION WARNING]: Distribution centos 8.2.1 on host 3l-hl-xe04 should use /usr/libexec/platform-python, but is using /usr/bin/python for backward compatibility with prior 
Ansible releases. A future Ansible release will default to using the discovered platform python for this host. See 
https://docs.ansible.com/ansible/2.10/reference_appendices/interpreter_discovery.html for more information. This feature will be removed in version 2.12. Deprecation warnings 
can be disabled by setting deprecation_warnings=False in ansible.cfg.
3l-hl-xe04 | CHANGED | rc=0 >>
          name-label ( RW): Ubuntu Bionic Beaver 18.04
          name-label ( RW): Ubuntu Xenial Xerus 16.04
          name-label ( RW): Ubuntu Jammy Jellyfish 22.04
          name-label ( RW): Ubuntu Focal Fossa 20.04
[DEPRECATION WARNING]: Distribution centos 8.2.1 on host 3l-hl-xe01 should use /usr/libexec/platform-python, but is using /usr/bin/python for backward compatibility with prior 
Ansible releases. A future Ansible release will default to using the discovered platform python for this host. See 
https://docs.ansible.com/ansible/2.10/reference_appendices/interpreter_discovery.html for more information. This feature will be removed in version 2.12. Deprecation warnings 
can be disabled by setting deprecation_warnings=False in ansible.cfg.
3l-hl-xe01 | CHANGED | rc=0 >>
          name-label ( RW): Ubuntu Focal Fossa 20.04
          name-label ( RW): Ubuntu Jammy Jellyfish 22.04
          name-label ( RW): Ubuntu Bionic Beaver 18.04
          name-label ( RW): 3l-xe01 Ubuntu 20.04 Cloud-Init 71
          name-label ( RW): Ubuntu Xenial Xerus 16.04
[DEPRECATION WARNING]: Distribution centos 8.2.1 on host 3l-hl-xe02 should use /usr/libexec/platform-python, but is using /usr/bin/python for backward compatibility with prior 
Ansible releases. A future Ansible release will default to using the discovered platform python for this host. See 
https://docs.ansible.com/ansible/2.10/reference_appendices/interpreter_discovery.html for more information. This feature will be removed in version 2.12. Deprecation warnings 
can be disabled by setting deprecation_warnings=False in ansible.cfg.
3l-hl-xe02 | CHANGED | rc=0 >>
          name-label ( RW): Ubuntu 20.04 Cloud-Init 71 xe02
          name-label ( RW): Ubuntu Jammy Jellyfish 22.04
          name-label ( RW): Ubuntu Focal Fossa 20.04
          name-label ( RW): Ubuntu Xenial Xerus 16.04
          name-label ( RW): Ubuntu Bionic Beaver 18.04
```
Sooooooooooo that's a pin for tonight. Can't get into the 7040 on the desk that's a standalone to see if I can get it that way.... tho My suspicions is that the bulk of the not made by someone templates are only provided via xoa template hub. So the auto generating a vm is still on t he docket... tho the projects I had as a reference was the right track. So mayhaps I need to temporarily disconnect xe03-04 from xoa to get this POC working. While also working on reinstalling ont he 7040 & researching ways to automate the install if possible. 

https://github.com/xcp-ng/host-installer

https://github.com/xcp-ng/xcp/issues/117

sinc eit's a custom installer script there seems to be an answer file that can be written & fed in
https://xcp-ng.org/docs/answerfile.html

