# WEEK 3 - 
# ALL-IN-ONE OPENSTACK DEPLOYMENT

## I. INFRASTRUCUTRE

## II. STEPS-BY-STEP

### A. SET UP ENIVRONMENT:

### 1. Update `apt` & install essentails dependencies:

```
$ sudo apt update

$ sudo apt install python3-dev libffi-dev gcc libssl-dev

```

### 2. Using `virtualenv`:
- Install `virtualenv`:
```
$ sudo apt install python3-venv
```

- Create `virutalenv` + activate:
```
$ python3 -m venv /path/to/venv

$ source /path/to/venv/bin/activate
```

### 3. Install `Ansible` & `Kolla-Ansible`:

- Install `ansible` (within `virtualenv`):
```
$ pip install 'ansible==2.9'
```

- Install `Kolla-ansible`:

```
$ pip install kolla-ansible
```

### B. CONFIGURE `Kolla-Ansible` & `Ansible`:

### 1. Create `/etc/kolla`:

```
$ sudo mkdir -p /etc/kolla
$ sudo chown $USER:$USER /etc/kolla
```

### 2. Copy `passwords.yml` to `/etc/kolla`:

```
$ cp -r <path-to-virtualenv>/share/kolla-ansible/etc_examples/kolla/* /etc/kolla
```

### 3. Configure `Ansible`:

```
$ mkdir -p /etc/ansible
$ txt="[defaults]\nhost_key_checking=False\npipelining=True\nforks=100"
$ echo -e $txt >> /etc/ansible/ansible.cfg

```

### C. PRE-DEPLOY CONFIGURATIONS:

### 1. Configure `all-in-one` (`inventory` file)
> Optional. Should user defaults for `all-in-one`

### 2. Run `ad-hoc` command checking configurations:
```
ansible -i all-in-one all -m ping
```

### 3. Create diskspace partition for `Cinder` (Block Storage):
```
$ sudo pvcreate /dev/sdb

$ sudo vgcreate cinder-volumes /dev/sdb
```

### 4. Generate Passwords for `Kolla`:
- Stored in `/etc/kolla/passwords.yml`:

```
$ kolla-genpwd
```

Or

```
$ cd kolla-ansible/tools
$ ./generate_passwords.py
```

### 5. Configure `globals.yml`:

```
$ vi /etc/kolla/globals.yml
```

**Example**:

```
kolla_base_distro: "ubuntu"
kolla_install_type: "source"

openstack_release: "train"

network_interface: ens33
neutron_external_interface: ens38

nova_compute_virt_type: "qemu"

enable_haproxy: "no"

enable_cinder: "yes"
enable_cinder_backup: "no"
enable_cinder_backend_lvm: "yes"

```

## D. DEPLOY
- Bootstrap:
```
$ kolla-ansible -i all-in-one bootstrap-servers
```

- Prechecks:
```
$ kolla-ansible -i all-in-one prechecks
```

- Pull Images:
```
$ kolla-ansible -i all-in-one pull
```

- Deploy:
```
$ kolla-ansible -i all-in-one deploy
```

- Post-deploy:
```
$ kolla-ansible -i all-in-one post-deploy
```

## E. POST-DEPLOYMENT:
- Install Openstack CLI:
```
$ pip install python-openstackclient python-glanceclient python-neutronclient
```


```
$ source /etc/kolla/admin-openrc.sh
```

```
$ openstack token issue
```

## IV. DEBUGGING:
### 1. `IPv4 not available for `ens38``:

- View all NICs & statuses:
```
$ ip r
```

- Remove old IP & Request new IP via `DHCP`:

```
$ sudo dhclient -r <nic-address>

$ sudo dhclient <nic-addr>
```

### 2. When boostrapping `cannot import name 'AnsibleCollectionLoader' from 'ansible.utils.collection_loader'`

- Remove existing `ansible` & `ansible-base` packages:

```
$ pip uninstall <package-name> 
```

- Install `Ansible==2.9.0`:
```
$ pip install 'ansible==2.9'
```

### 3. `admin-openrc.sh` Missing:

