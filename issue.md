[Fedora Rawhide] Failed to download linux-6.8.0.tar.xz

## Issue

I am experimenting with installing the driver on [Fedora Rawhide](https://docs.fedoraproject.org/en-US/releases/rawhide/) (Fedora's development version). The goal is to ultimately add the driver to the official repo (or as a [copr](https://docs.fedoraproject.org/en-US/neurofedora/copr/)).

The driver is packaged in the AUR (thx to @huntekye), so I think this is doable.

- https://github.com/davidjo/snd_hda_macbookpro/pull/105

[![Packaging status](https://repology.org/badge/vertical-allrepos/snd-hda-macbookpro-dkms.svg)](https://repology.org/project/snd-hda-macbookpro-dkms/versions)

Currently, I get the following error:

```console
$ sudo ./install.cirrus.driver.sh 
Ensure the patch package is installed
0 files              100% [=============================================================================================================>]     265     --.-KB/s
                          [Files: 0  Bytes: 265  [949 B/s] Redirects: 0  Todo: 0  Errors: 1                                              ]
Failed to download linux-6.8.0.tar.xz
Trying to download base kernel version linux-6.8.tar.xz
This may lead to build failures as too old
If this is an Ubuntu-based distribution this almost certainly will fail to build

0 files              100% [=============================================================================================================>]     265     --.-KB/s
                          [Files: 0  Bytes: 265  [939 B/s] Redirects: 0  Todo: 0  Errors: 1                                              ]
kernel could not be downloaded...exiting
```

The reason is that `wget -c https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.8.0.tar.xz` fails.

As of writing this issue, https://mirrors.edge.kernel.org/pub/linux/kernel/v6.x has up to `6.7.9`:

    linux-6.7.9.tar.gz                                 06-Mar-2024 15:09    218M

## Suggested Solution

Is there a way to find an alternative link that has `linux-6.8.0.tar.xz` (the latest preview release)?

## Verbose Logs

```console
$ uname -a
Linux fedora 6.8.0-0.rc7.20240307git67be068d31d4.59.fc41.x86_64 #1 SMP PREEMPT_DYNAMIC Thu Mar  7 17:58:13 UTC 2024 x86_64 GNU/Linux
```

```console
$ sudo bash -x ./install.cirrus.driver.sh 
+ set -e
+ '[' 0 -gt 0 ']'
++ uname -r
+ UNAME=6.8.0-0.rc7.20240307git67be068d31d4.59.fc41.x86_64
++ echo 6.8.0-0.rc7.20240307git67be068d31d4.59.fc41.x86_64
++ cut -d - -f1
+ kernel_version=6.8.0
++ echo 6.8.0
++ cut -d . -f1
+ major_version=6
++ echo 6.8.0
++ cut -d . -f2
+ minor_version=8
+ major_minor=68
++ echo 6.8.0-0.rc7.20240307git67be068d31d4.59.fc41.x86_64
++ cut -d . -f3
+ revision=0-0
++ echo 0-0
++ cut -d - -f1
+ revpart1=0
++ echo 0-0
++ cut -d - -f2
+ revpart2=0
++ echo 0-0
++ cut -d - -f3
+ revpart3=
+ '[' 6 -eq 5 -a 8 -lt 13 ']'
+ sed -i 's/^BUILT_MODULE_NAME\[0\].*$/BUILT_MODULE_NAME[0]="snd-hda-codec-cs8409"/' dkms.conf
+ PATCH_CIRRUS=false
+ [[ '' == \i\n\s\t\a\l\l ]]
+ [[ '' == \r\e\m\o\v\e ]]
+ '[' 6 == 4 ']'
+ '[' 6 -eq 5 -a 8 -lt 8 ']'
+ isdebian=0
+ isfedora=0
+ isarch=0
+ isvoid=0
+ '[' -d /usr/src/linux-headers-6.8.0-0.rc7.20240307git67be068d31d4.59.fc41.x86_64 ']'
+ '[' -d /usr/src/kernels/6.8.0-0.rc7.20240307git67be068d31d4.59.fc41.x86_64 ']'
+ isfedora=1
+ :
+++ dirname -- ./install.cirrus.driver.sh
++ cd -- .
++ pwd
+ cur_dir=/home/nebula/git/snd_hda_macbookpro
+ build_dir=build
+ patch_dir=/home/nebula/git/snd_hda_macbookpro/patch_cirrus
+ hda_dir=/home/nebula/git/snd_hda_macbookpro/build/hda
+ update_dir=/lib/modules/6.8.0-0.rc7.20240307git67be068d31d4.59.fc41.x86_64/updates
+ [[ -d /home/nebula/git/snd_hda_macbookpro/build/hda ]]
+ [[ ! -d build ]]
+ '[' 1 -ge 1 ']'
+ echo 'Ensure the patch package is installed'
Ensure the patch package is installed
++ command -v patch
+ [[ ! -n /usr/bin/patch ]]
+ isubuntu=0
++ grep '^NAME=' /etc/os-release
++ grep -c Ubuntu
+ '[' 0 -eq 1 ']'
++ grep '^NAME=' /etc/os-release
++ grep -c 'Linux Mint'
+ '[' 0 -eq 1 ']'
+ '[' 0 -ge 1 ']'
+ set +e
+ wget -c https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.8.0.tar.xz -P build
0 files              100% [=============================================================================================================>]     265     --.-KB/s
                          [Files: 0  Bytes: 265  [1.49KB/s] Redirects: 0  Todo: 0  Errors: 1                                             ]
+ [[ 8 -ne 0 ]]
+ echo 'Failed to download linux-6.8.0.tar.xz'
Failed to download linux-6.8.0.tar.xz
+ echo 'Trying to download base kernel version linux-6.8.tar.xz'
Trying to download base kernel version linux-6.8.tar.xz
+ echo 'This may lead to build failures as too old'
This may lead to build failures as too old
+ echo 'If this is an Ubuntu-based distribution this almost certainly will fail to build'
If this is an Ubuntu-based distribution this almost certainly will fail to build
+ echo ''

+ kernel_version=6.8
+ wget -c https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.8.tar.xz -P build
0 files              100% [=============================================================================================================>]     265     --.-KB/s
                          [Files: 0  Bytes: 265  [1.46KB/s] Redirects: 0  Todo: 0  Errors: 1                                             ]
+ [[ 8 -ne 0 ]]
+ echo 'kernel could not be downloaded...exiting'
kernel could not be downloaded...exiting
+ exit
```

```console
$ fastfetch 
             .',;::::;,'.                 nebula@fedora
         .';:cccccccccccc:;,.             -------------
      .;cccccccccccccccccccccc;.          OS: Fedora Linux 41 (Workstation Edition) x86_64
    .:cccccccccccccccccccccccccc:.        Host: MacBookPro14,1 (1.0)
  .;ccccccccccccc;.:dddl:.;ccccccc;.      Kernel: 6.8.0-0.rc7.20240307git67be068d31d4.59.fc41.x86_64
 .:ccccccccccccc;OWMKOOXMWd;ccccccc:.     Uptime: 7 mins
.:ccccccccccccc;KMMc;cc;xMMc;ccccccc:.    Packages: 1998 (rpm), 6 (flatpak)
,cccccccccccccc;MMM.;cc;;WW:;cccccccc,    Shell: bash 5.2.26
:cccccccccccccc;MMM.;cccccccccccccccc:    Display (Color LCD): 2560x1600 @ 60Hz (as 1280x800) [Built-in]
:ccccccc;oxOOOo;MMM000k.;cccccccccccc:    DE: Gnome 46.rc
cccccc;0MMKxdd:;MMMkddc.;cccccccccccc;    WM: Mutter (Wayland)
ccccc;XMO';cccc;MMM.;cccccccccccccccc'    WM Theme: Adwaita
ccccc;MMo;ccccc;MMW.;ccccccccccccccc;     Theme: Adwaita [GTK2/3/4]
ccccc;0MNc.ccc.xMMd;ccccccccccccccc;      Icons: Adwaita [GTK2/3/4]
cccccc;dNMWXXXWM0:;cccccccccccccc:,       Font: Cantarell (11pt) [GTK2/3/4]
cccccccc;.:odl:.;cccccccccccccc:,.        Cursor: Adwaita (24px)
ccccccccccccccccccccccccccccc:'.          Terminal: GNOME Terminal 3.50.1
:ccccccccccccccccccccccc:;,..             Terminal Font: Source Code Pro (10pt)
 ':cccccccccccccccc::;,.                  CPU: Intel(R) Core(TM) i5-7360U (4) @ 3.60 GHz
                                          GPU: Intel Iris Plus Graphics 640
                                          Memory: 3.02 GiB / 7.62 GiB (40%)
                                          Swap: 0 B / 7.62 GiB (0%)
                                          Disk (/): 5.84 GiB / 17.62 GiB (33%) - btrfs
                                          Local IP (wlp2s0): 10.137.88.91/18 *
                                          Battery: 1% [Charging]
                                          Locale: en_US.UTF-8

                                          ████████████████████████
                                          ████████████████████████
```
