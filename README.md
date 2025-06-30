ansible-role-syncthing
======================
Install [Syncthing](https://syncthing.net/).

Requirements
------------
A Debian-based distribution, root or _become_ on the remote host.
Look at the [Syncthing documentation](https://docs.syncthing.net/users/firewall.html)
to see what ports to open. The firewall configuration is not handled in this role.

Role Variables
--------------
All variables are optional.  
The only way to set Syncthing options is to edit its `config.xml` file, see the
[Example Playbook](#example-playbook) section for details.

```yaml
... to update
```

Dependencies
------------
None.

Example Playbook
----------------
```yaml
# Install Syncthing
- hosts: servers
  roles:
    - { role: syncthing }
```

The Syncthing configuration is dynamic and edited at runtime using the GUI.  
You will need to get the configuration from the host and write it in the
`syncthing_config_*` variables if you want to keep it safe and ensure it is the
configuration you actually have on the server next time you run the role.

**Failing to fetch the configuration before running the role again means that
all new devices/folders/configuration will be erased from the remote host.**

If you don't set the `syncthing_config_*` variables nothing will be overwritten
but you will lose your configuration if your server sets itself on fire.

So make sure to back that up. From the config dir, {{syncthing_config_directory}}.

You can use `lookup` to read the files and set the variables:

```yaml
syncthing_config_cert: "{{lookup('file', 'files/{{ inventory_hostname }}/home/syncthing/{{syncthing_config_directory}}/cert.pem')}}"
syncthing_config_key: "{{lookup('file', 'files/{{ inventory_hostname }}/home/syncthing/{{syncthing_config_directory}}/key.pem')}}"
syncthing_config_https_cert: "{{lookup('file', 'files/{{ inventory_hostname }}/home/syncthing/{{syncthing_config_directory}}/https-cert.pem')}}"
syncthing_config_https_key: "{{lookup('file', 'files/{{ inventory_hostname }}/home/syncthing/{{syncthing_config_directory}}/https-key.pem')}}"
syncthing_config_config: "{{lookup('file', 'files/{{ inventory_hostname }}/home/syncthing/{{syncthing_config_directory}}/config.xml')}}"
```

I advise to use `ansible-vault` to encrypt the keys, `lookup` will decrypt them
on the fly.

License
-------
MIT
