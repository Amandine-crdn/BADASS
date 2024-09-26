# BADASS
Creer sa VM
Install docker
Install GNS3: https://superuser.com/questions/1788720/how-to-install-gns3-on-debian12

## P1:
Lancer gns3
Creer les images docker
Creer un projet sur gns3 dans son p1
Importer les images docker

Install busybox

# P2

## Static

### Host
```bash
ip addr add 30.1.1.1/24 dev eth1
ip link set eth1 up
```

### Router
```bash
ip addr add 10.1.1.X/24 dev eth0 
```

```bash
ip link add br0 type bridge 
ip link set dev br0 up
```

```bash
ip link add name vxlan10 type vxlan id 10 dev eth0 remote 10.1.1.X local 10.1.1.Y dstport 4789 
ip addr add 20.1.1.1/24 dev vxlan10 
ip link set dev vxlan10 up 
```

```bash
brctl addif br0 eth1 
brctl addif br0 vxlan10
```

## Multicast

### Host
```bash
ip addr add 30.1.1.X/24 dev eth1
ip link set eth1 up
```

### Router
```bash
ip addr add 10.1.1.X/24 dev eth0 
```

```bash
ip link add br0 type bridge 
ip link set dev br0 up
```

```bash
ip link add name vxlan10 type vxlan id 10 dev eth0 group 239.1.1.1 dstport 4789 
ip addr add 20.1.1.1/24 dev vxlan10 
ip link set dev vxlan10 up 
```

```bash
brctl addif br0 eth1 
brctl addif br0 vxlan10
```

## commands check 
```` bash
ip link show vxlan10 
ip link show ethX
ip link show

brctl showmacs br0
```` 
## or insert into a config file

auto eth0
iface eth0 inet static
    address 10.1.1.1
    netmask 255.255.255.0

auto br0
iface br0 inet manual
    bridge_ports eth1 vxlan10
    pre-up ip link add br0 type bridge
    up ip link set br0 up

iface vxlan10 inet static
    address 20.1.1.1
    netmask 255.255.255.0
    pre-up ip link add name vxlan10 type vxlan id 10 dev eth0 remote 10.1.1.1 local 10.1.1.2 dstport 4789
    up ip link set vxlan10 up
