
# Ansible Role - potos\_openvpn

This role works for me, but does not satisfy the potos acceptance rules yet:

* the automated checks are currently failing
* meta is not up to date

This role installs `network-manager-openvpn-gnome` (and all dependencies).

If `potos_openvpn_install_user_handover` is set to `true`, two scripts
and a `sudo` rule are installed that allow to configure a system
vpn connection from a unprivileged account.

The user can call `activate_vpn` providing as input a gpg encrypted
`.ovpn` configuration file. The script asks first for the
decryption password and then for the `sudo` password to actually
configure the vpn systemwide. The connection is established as soon
as the system has connectivity. Of course the `.ovpn` file must
not require additional passwords to decrypt e.g. the private key.

In my use case:

* The always on VPN connection is very limited: no default route, not even DNS.
* Firewalled on the VPN server end to prohibit almost everything.
* I (as the administrator) use the connection to be able to login
  to any family computer as soon it is connected to the internet.

If you don't want to install the VPN system wide but have a
valid `.ovpn` file, use the following command

```
nmcli connection import type openvpn file your-openvpn-config-file.ovpn
```

This creates a new (manually to be started/stopped) VPN connection
for NetworkManager.

## Example Playbook

As this role is tested via Molecule one can use [that
playbook](./molecule/default/converge.yml) as a starting point:

```yaml
---

- name: Converge
  hosts: all
  gather_facts: yes
  tasks:
    - name: run role
      ansible.builtin.include_role:
        name: 'ansible-role-potos_template'
```

## Role Variables

The default variables are defined in [defaults/main.yml](./defaults/main.yml):

```yaml
---

potos_openvpn_install_user_handover: true
```

Another option is to use `ansible-doc` to read the argument specification:

```sh
ansible-doc --type role -r . main ansible-role-potos_template
```

## Requirements

N/A

## License

See [LICENSE](./LICENSE)

## Author Information

[Project Potos](https://github.com/projectpotos)

