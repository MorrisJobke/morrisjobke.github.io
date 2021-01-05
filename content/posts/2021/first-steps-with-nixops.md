---
title:  "First steps with NixOps"
date:   "2021-01-05T22:15:00Z"
categories:
- howto
---

I wanted to play around a bit with [NixOS](https://nixos.org) and [NixOps](https://nixos.org/nixops). Those are the notes on how I got started with it on macOS and an VM inside VirtualBox.

## Requirements

You need to have `nixops` installed locally. I installed [nix](https://nixos.org) on macOS Big Sur via [the tutorial on Philipps blog post](https://www.philipp.haussleiter.de/2020/04/fixing-nix-setup-on-macos-catalina/) and then ran `nix-env -i nixops`.

## Configuration files

The following two configuration files are needed for a first simple [NixOps](https://nixos.org/nixops) setup.

`trivial.nix`

```
{
  network.description = "Web server";

  webserver =
    { config, pkgs, ... }:
    { services.httpd.enable = true;
      services.httpd.adminAddr = "alice@example.org";
      networking.firewall.allowedTCPPorts = [ 80 ];

      services.httpd.logFormat = "combined";
      services.httpd.virtualHosts = {
        "main" = {
          documentRoot = "/tmp";
        };
      };
    };
}
```

`trivial-vbox.nix`
```
{
  webserver =
    { config, pkgs, ... }:
    { deployment.targetEnv = "virtualbox";
      deployment.virtualbox.memorySize = 1024; # megabytes
      deployment.virtualbox.vcpu = 2; # number of cpus
      deployment.virtualbox.headless = true;
    };
}
```

## Deploying it

Create deployment:
```
$ nixops create ./trivial.nix ./trivial-vbox.nix -d trivial
```

Show deployment state:
```
$ nixops info -d trivial
Network name: trivial
Network UUID: 89dbba3d-4f9c-11eb-b54e-acbc32a44755
Network description: Web server
Nix expressions: /Users/morris/Projects/nixops-machines/trivial.nix /Users/morris/Projects/nixops-machines/trivial-vbox.nix

+-----------+---------+------------+-------------+------------+
| Name      |  Status | Type       | Resource Id | IP address |
+-----------+---------+------------+-------------+------------+
| webserver | Missing | virtualbox |             |            |
+-----------+---------+------------+-------------+------------+
```

Deploy machine (that creates the VM and deploys the NixOS configuration):
```
$ nixops deploy -d trivial
webserver> creating VirtualBox VM...
webserver> Virtual machine 'nixops-89dbba3d-4f9c-11eb-b54e-acbc32a44755-webserver' is created and registered.
webserver> UUID: db550623-f02f-4e8c-a0c8-e79ec7dee325
webserver> Settings file: '/Users/morris/VirtualBox VMs/nixops-89dbba3d-4f9c-11eb-b54e-acbc32a44755-webserver/nixops-89dbba3d-4f9c-11eb-b54e-acbc32a44755-webserver.vbox'
webserver> creating disk ‘disk1’...
webserver> these derivations will be built:
webserver>   /nix/store/rr7p21npwab33dyf0aph8vlkqqhvkxlk-virtualbox-nixops-19.03.172205.ea497998e4b.vmdk.xz.drv
webserver>   /nix/store/sw3bp8vnc3m2cp0lfv1pfjm2kk9p9pvj-virtualbox-nixops-21.03.vmdk.drv
webserver> these paths will be fetched (2.87 MiB download, 9.22 MiB unpacked):
webserver>   /nix/store/01pkcbbpl13rrvgxlhcbxyfbsi88g56s-curl-7.74.0
webserver>   /nix/store/1cvgyzi5hz4ni0cz5pv09frgq6lln9i4-stdenv-darwin
webserver>   /nix/store/301q4qwn3rvl9b8l0c5296bgrf9hb914-curl-7.74.0-bin
webserver>   /nix/store/48mk2wkwhrhpcfzsr23kbh25lkirhwih-openssl-1.1.1i-bin
...
```

Check the machine state:
```
$ nixops check -d trivial
Machines state:
+-----------+--------+-----+-----------+----------+----------------+------------------------------------------+-------+
| Name      | Exists | Up  | Reachable | Disks OK | Load avg.      | Units                                    | Notes |
+-----------+--------+-----+-----------+----------+----------------+------------------------------------------+-------+
| webserver | Yes    | Yes | Yes       | N/A      | 0.02 0.16 0.14 | ● home.mount [failed]                    |       |
|           |        |     |           |          |                |   sys-fs-fuse-connections.mount [failed] |       |
|           |        |     |           |          |                |   sys-kernel-config.mount [failed]       |       |
|           |        |     |           |          |                | ● tmp.mount [failed]                     |       |
+-----------+--------+-----+-----------+----------+----------------+------------------------------------------+-------+
Non machines resources state:
+------+--------+
| Name | Exists |
+------+--------+
+------+--------+
```


## Links

* [NixOps 1.7 manual](https://releases.nixos.org/nixops/nixops-1.7/manual/manual.html)
* [Was NixOS kann - Ein Webserver-Beispiel (noqqe.de)](https://noqqe.de/blog/2020/06/24/nixos-httpd/)
* [Fixing nix Setup on MacOS Catalina](https://www.philipp.haussleiter.de/2020/04/fixing-nix-setup-on-macos-catalina/)