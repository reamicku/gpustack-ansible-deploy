# GPUStack deployment on multi-node cluster with Ansible

- [GPUStack deployment on multi-node cluster with Ansible](#gpustack-deployment-on-multi-node-cluster-with-ansible)
  - [About](#about)
  - [Setting up a local environment with Libvirt](#setting-up-a-local-environment-with-libvirt)
    - [Prepare virtual machines](#prepare-virtual-machines)
    - [Set DHCP leashes for virtual machines](#set-dhcp-leashes-for-virtual-machines)
  - [SSH keys](#ssh-keys)
  - [Playbooks](#playbooks)
    - [Service](#service)
  - [License](#license)

## About

This ansible project automatically installs [GPUStack](https://github.com/gpustack/gpustack) on target machines.

The plan is described in [PLAN.md](./PLAN.md).

## Setting up a local environment with Libvirt

### Prepare virtual machines

If you want to use Libvirt as your backend for this project, then read this section. If not, then feel free to skip to the [SSH Keys](#ssh-keys) section.

Create 4 debian virtual machines. Recommended system requirements for each of them is as follows:

| Resource      | Value |
| ------------- | ----- |
| CPU Cores     | 2     |
| RAM           | 4 GB  |
| Storage space | 20 GB |

Install `openssh-server` package on each of them.

### Set DHCP leashes for virtual machines

Add those lines in the network definition in Virtual Machine Manager (VMM):

```xml
    ...
    <dhcp>
      ...
      <host mac="52:54:00:49:19:c5" ip="192.168.122.10"/>
      <host mac="52:54:00:e7:05:b0" ip="192.168.122.21"/>
      <host mac="52:54:00:a3:f7:80" ip="192.168.122.22"/>
      <host mac="52:54:00:a1:69:da" ip="192.168.122.23"/>
    </dhcp>
```

Match the MAC addresses with the virtual machine MAC addresses.

Proceed to stop and start the network while all machines are off.

Now your virtual environment is ready to be used by ansible.

## SSH keys

Generate ssh key for ansible to connect to hosts.

```bash
mkdir keys
ssh-keygen -t ed25519 -f ./keys/user_key -C "user@example.com"
```

Copy over ssh keys for each machine:

```bash
ssh-copy-id -o PubkeyAuthentication=no -i ./keys/user_key.pub user@192.168.122.10
ssh-copy-id -o PubkeyAuthentication=no -i ./keys/user_key.pub user@192.168.122.21
ssh-copy-id -o PubkeyAuthentication=no -i ./keys/user_key.pub user@192.168.122.22
ssh-copy-id -o PubkeyAuthentication=no -i ./keys/user_key.pub user@192.168.122.23
```

## Playbooks

Connection test:

```bash
ansible-playbook playbooks/ping.yml
```

Deploy GPUStack on nodes and start it:

```bash
ansible-playbook playbooks/site.yml
```

Start GPUStack:

```bash
ansible-playbook playbooks/start.yml
```

Stop GPUStack:

```bash
ansible-playbook playbooks/stop.yml
```

### Service

The service will be available at the address printed out after the `playbooks/site.yml` playbook finishes deploying.

By default the service is exposed on the controller node on port `6712` (unless changed in `inventory.ini` file).

Default credentials:

- User: `admin`
- Password: `admin12345`

## License

[MIT License](./LICENSE)
