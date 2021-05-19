# WEEK-3 PRACTICES DOCUMENTATION
# OPENVSWITCH

---

## **Author:** *Julian (Phong) Ng.* 
**Date of issue**: *May 20th 2021*

> Welcome back! This is the documentation for my second training project at **Viettel Network**. Enjoy ur time :smile_cat:. Feel free to hit me up if any edition is needed!

---
# **Table of Contents:** TODO

## [I. Overview](#**I.-OVERVIEW**)

## [II. Step-by-step](#**I.-OVERVIEW**)

### Env

## [III. References](#V.-REFERENCES)

## SET UP
- Network interfaces: `Host-only`

# **I. OVERVIEW**:
## CONCEPTS:
- `Overlay Network`
- VXLAN:
	- Create a phys L2 over IP
	- Establishing connection via `tunnel` -> extends L2 on L3
	- `VTEP`: 
		+ VXLAN Tunnel End Point -> stays on Hypervisor on host of VMs
		+ Can be on switch/ phys server or phys/software


# **II. STEP-BY-STEP**:

## **A. SET UP ENVIRONMENT**:
- `Must-have` packages: `net-tools`, `tcpdump`
```
$ sudo apt install net-tools
```

- Installing `OpenVswitch` via `apt`:
```
$ sudo apt update
$ sudo apt upgrade
$ sudo apt install openvswitch-switch
```

- Starting `OpenVswitch`:
```
$ sudo ovs-vswitchd
```

- Check status of service:
```
$ systemctl status ovs-vswitchd
```

## **B. CONFIGURE HOSTS**:

### **1. Host 1**:
- Adding Bridges:
```
$ sudo ovs-vsctl add-br br0
$ sudo ovs-vsctl add-br br1
```

```
$ sudo ovs-vsctl add-port br0 ens33
```

```
# (removes) the IP address assigned to interface ens33
$ sudo ifconfig ens33 0 && sudo ifconfig br0 192.168.50.130 netmask 255.255.255.0

$ sudo route add default gw 192.168.50.1 br0

$ sudo ifconfig br1 10.1.3.10 netmask 255.255.255.0

$ sudo ovs-vsctl add-port br1 vxlan1 -- set interface vxlan1 type=vxlan options:remote_ip=192.168.50.128
```

### 2. HOST 2:
```
$ sudo ovs-vsctl add-br br0

$ sudo ovs-vsctl add-br br1

$ sudo ovs-vsctl add-port br0 ens33
```

```
# (removes) the IP address assigned to interface ens33
$ sudo ifconfig ens33 0 && sudo ifconfig br0 192.168.50.128 netmask 255.255.255.0

$ sudo route add default gw 192.168.50.1 br0

$ sudo ifconfig br1 10.1.3.11 netmask 255.255.255.0

$ sudo ovs-vsctl add-port br1 vxlan1 -- set interface vxlan1 type=vxlan options:remote_ip=192.168.50.130

```

## B. CHECK CONNECTION BETWEEN 2 NODES:
### 1. `tcpdump`:

### 2. `wiretshark`:

## Q/A Section:

## REFERENCES:
- [Configuring VXLAN & GRE Tunnel on openvswitch](http://networkstatic.net/configuring-vxlan-and-gre-tunnels-on-openvswitch/)

- [VXLAN w/ OpenvSwitch](https://github.com/hocchudong/thuctap012017/blob/master/XuanSon/Netowork%20Protocol/VXLAN-GRE%20Protocol.md)

