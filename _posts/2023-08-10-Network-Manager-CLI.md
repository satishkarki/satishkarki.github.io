---
layout: post
title: "nmcli - network manager CLI"
date: 2023-08-10
categories: Linux
---

------

Network Manager Command Line Interface is a command line utility for controlling NetworkManager and reporting network status.

```bash
nmcli [OPTIONS] OBJECT { COMMAND | help }
```

```bash
butcher@butcher:~$ nmcli help
Usage: nmcli [OPTIONS] OBJECT { COMMAND | help }

OPTIONS
  -a, --ask                                ask for missing parameters
  -c, --colors auto|yes|no                 whether to use colours in output
  -e, --escape yes|no                      escape columns separators in values
  -f, --fields <field,...>|all|common      specify fields to output
  -g, --get-values <field,...>|all|common  shortcut for -m tabular -t -f
  -h, --help                               print this help
  -m, --mode tabular|multiline             output mode
  -o, --overview                           overview mode
  -p, --pretty                             pretty output
  -s, --show-secrets                       allow displaying passwords
  -t, --terse                              terse output
  -v, --version                            show program version
  -w, --wait <seconds>                     set timeout waiting for finishing operations

OBJECT
  g[eneral]       NetworkManager's general status and operations
  n[etworking]    overall networking control
  r[adio]         NetworkManager radio switches
  c[onnection]    NetworkManager's connections
  d[evice]        devices managed by NetworkManager
  a[gent]         NetworkManager secret agent or polkit agent
  m[onitor]       monitor NetworkManager changes

```

| command | function |
| -------- | ------- |
| nmcli -p device| -p pretty view of device status|
| nmcli device status | All devices and their status|
| nmcli connection show | List all configured connections |
| nmcli connection show --active | Active connections only |
| nmcli device wifi show-password | displays SSID, security type and password with QR code |
| nmcli general status | Network Manager Status |

### `nmcli -p device`
```bash
butcher@butcher:~$ nmcli -p device
=====================
Status of devices
=====================
DEVICE           TYPE      STATE                   CONNECTION 
------------------------------------------------------------------------------------
wlx1cbfce718a5d  wifi      connected               MacSheen   
lo               loopback  connected (externally)  lo         
eno1             ethernet  unavailable             --         
enp2s0f0         ethernet  unavailable             --         
enp2s0f1         ethernet  unavailable             --         
butcher@butcher:~$ 
```
### `nmcli connection show`
```bash
butcher@butcher:~$ nmcli connection show
NAME                UUID                                  TYPE      DEVICE     >
MacSheen            01d7db20-0b63-41ab-b929-eb9b0c70f2cf  wifi      wlx1cbfce71>
lo                  a3b523b8-9d75-4b39-bf19-463281474490  loopback  lo         >
Hifi                78307044-52f3-46fc-918f-47bb1cfeff97  wifi      --         >
MacSheen 1          d6a02df1-3668-48e1-b0d4-3834f0c3946e  wifi      --         >
shanker             22d1027a-c380-4a79-bbc2-f4fd849a424d  wifi      --         >
Wired connection 1  27e5b3d3-6899-3ccb-93dd-e611ed2d3ee7  ethernet  --         >
Wired connection 2  9c27ebb5-05c3-32b7-82a6-9f468b4f3157  ethernet  --         >
Wired connection 3  0d70d260-d400-3dca-985f-224ad2f7942c  ethernet  --         >
```
### `nmcli connection show --active`
```bash
butcher@butcher:~$ nmcli connection show --active
NAME      UUID                                  TYPE      DEVICE          
MacSheen  01d7db20-0b63-41ab-b929-eb9b0c70f2cf  wifi      wlx1cbfce718a5d 
lo        a3b523b8-9d75-4b39-bf19-463281474490  loopback  lo              
```
### `nmcli device wifi show-password`
This one is my favorite

```bash
butcher@butcher:~$ nmcli device wifi show-password
SSID: MacSheen
Security: **********
Password: **********

  █████████████████████████████████
  ██ ▄▄▄▄▄ █▀█ █▄   ▀█▀█ █ ▄▄▄▄▄ ██
  ██ █   ▀▀▀█ ▄▀ █▀  ▀█ █   █ ██
  ██ █▄█ █▀ █▀▀██▄▄ █▄▄▄█ ██
  ██▄▄▄▄▄▄▄█▄▀ ▀▄█ █ █ ▀ █▄▄▄▄▄▄▄██
  ██ ▄▄ ▄▀▄▄▄▄▀▄▀▀▀▀█▄█ ▀▄▀▄▀▄▀ ███
  ██▀█  ██▄▀▄█▄█▀ ▄▄▄█▄█   ▄▀▄█▀▀██
  ██▀█▄▀▄█▄█▄ ▄█▄█▄█▄▄█▀▀▀█  ▄ ▀███
  ██▄ ▄█▄▄▄ ▄▀  ▄█▀▄█▀▄████
  ██ ▀▄▄██▄█▀█ ▄▀▀▀█▄▄▀▀▀▀▀▄▀ █ ▄██
  ██ █▄█▄▄▄▄█ ██▀ ▄ ▄█▄██▄█▄ ███▀██
  ██▄██▄▄█▄█ ███▄█▄▄▄ ▀▄ ▄▄▄ ▄ ████
  ██ ▄▄▄▄▄ █▄▄█ ▄█▀  ▄▀  █▄█ ▀▄▀▀██
  ██ █   █ █ █ ▄▀▀▀█▀▄█▀ ▄▄▄  █ ▀██
  ██ █▄▄▄█ █ ▄▀█▀ ▄  █▀▀█ ▄▄  ▄ ███
  ██▄▄▄▄▄▄▄█▄█▄█▄█▄▄▄▄█▄██▄█▄▄█████
  ▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀

butcher@butcher:~$ 
```
### `nmcli general status`
```bash
butcher@butcher:~$ nmcli general status
STATE      CONNECTIVITY  WIFI-HW  WIFI     WWAN-HW  WWAN    
connected  full          enabled  enabled  missing  enabled 
```
## Reference
https://networkmanager.dev/docs/api/latest/nmcli.html

https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/networking_guide/sec-configuring_ip_networking_with_nmcli
