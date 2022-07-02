インストール後
$ssh root@ipaddress

$ sudo apt-get update
$ sudo apt-get -y install sudo

ssh-keygen.exe -q -t ed25519 -C '""' -N '""' -f id_ed25519
wsl
 ssh-copy-id -i ~/.ssh/id_ed25519.pub root@[ip] (windowsとwslで.sshを共有していることが条件 ln -s chmod -R 600 )
ログイン

$ ssh root@192.168.0.11
"""
Linux Alice 5.15.30-2-pve #1 SMP PVE 5.15.30-3 (Fri, 22 Apr 2022 18:08:27 +0200) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sat Jul  2 14:01:16 2022 from 192.168.0.146
root@Alice:~#
"""
echo "deb [arch=amd64] http://download.proxmox.com/debian/pve bullseye pve-no-subscription" > /etc/apt/sources.list.d/pve-install-repo.list

nano /etc/apt/sources.list

nano  nano /etc/apt/sources.list.d/pve-enterprise.list

"""
-deb https://enterprise.proxmox.com/debian/pve bullseye pve-enterprise
+#deb https://enterprise.proxmox.com/debian/pve bullseye pve-enterprise
"""


wget https://enterprise.proxmox.com/debian/proxmox-release-bullseye.gpg -O /etc/apt/trusted.gpg.d/proxmox-release-bullseye.gpg 
sha512sum /etc/apt/trusted.gpg.d/proxmox-release-bullseye.gpg

"""
--2022-07-02 14:35:05--  https://enterprise.proxmox.com/debian/proxmox-release-bullseye.gpg
Resolving enterprise.proxmox.com (enterprise.proxmox.com)... 51.79.159.216, 2402:1f00:8000:800::467
Connecting to enterprise.proxmox.com (enterprise.proxmox.com)|51.79.159.216|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1187 (1.2K) [application/octet-stream]
Saving to: ‘/etc/apt/trusted.gpg.d/proxmox-release-bullseye.gpg’

/etc/apt/trusted.gpg.d/proxmo 100%[=================================================>]   1.16K  --.-KB/s    in 0s

2022-07-02 14:35:06 (50.4 MB/s) - ‘/etc/apt/trusted.gpg.d/proxmox-release-bullseye.gpg’ saved [1187/1187]

7fb03ec8a1675723d2853b84aa4fdb49a46a3bb72b9951361488bfd19b29aab0a789a4f8c7406e71a69aabbc727c936d3549731c4659ffa1a08f44db8fdcebfa  /etc/apt/trusted.gpg.d/proxmox-release-bullseye.gpg
"""

apt update
apt upgrade -y && apt dist-upgrade -y

lsblk
"""
NAME               MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda                  8:0    0 223.6G  0 disk
├─sda1               8:1    0  1007K  0 part
├─sda2               8:2    0   512M  0 part /boot/efi
└─sda3               8:3    0 223.1G  0 part
  ├─pve-swap       253:0    0     8G  0 lvm  [SWAP]
  ├─pve-root       253:1    0  55.8G  0 lvm  /
  ├─pve-data_tmeta 253:2    0   1.4G  0 lvm
  │ └─pve-data     253:4    0 140.5G  0 lvm
  └─pve-data_tdata 253:3    0 140.5G  0 lvm
    └─pve-data     253:4    0 140.5G  0 lvm
"""
https://qiita.com/miyuki_samitani/items/ee3f93fa82d6ada1fa37

lvdisplay
"""
--- Logical volume ---
  LV Name                data
  VG Name                pve
  LV UUID                aZXTxc-0Lf2-PnEl-fO0d-5p8v-BvUz-G4OdAE
  LV Write Access        read/write
  LV Creation host, time proxmox, 2022-07-02 00:24:09 +0900
  LV Pool metadata       data_tmeta
  LV Pool data           data_tdata
  LV Status              available
  # open                 0
  LV Size                140.45 GiB
  Allocated pool data    0.00%
  Allocated metadata     1.14%
  Current LE             35956
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:4
"""

lvremove /dev/pve/data

lvextend -r -L 103.28G /dev/pve/root <=最終的なサイズ
lvreduce -L サイズ 論理ボリューム名
などなど


windowsinstall
https://zenn.dev/northeggman/articles/49c6b73c03c81c#%E7%92%B0%E5%A2%83

https://www.microsoft.com/ja-jp/software-download/windows11
https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso


すぐコンソールは言って任意のキー連打

iommu
https://pve.proxmox.com/wiki/Pci_passthrough#Enable_the_IOMMU

#!/bin/bash
shopt -s nullglob
for d in /sys/kernel/iommu_groups/*/devices/*; do 
    n=${d#*/iommu_groups/*}; n=${n%%/*}
    printf 'IOMMU Group %s ' "$n"
    lspci -nns "${d##*/}"
done;

IOMMU Group 0 00:00.0 Host bridge [0600]: Intel Corporation Device [8086:4650] (rev 02)
IOMMU Group 1 00:01.0 PCI bridge [0604]: Intel Corporation Device [8086:460d] (rev 02)
IOMMU Group 2 00:08.0 System peripheral [0880]: Intel Corporation Device [8086:464f] (rev 02)
IOMMU Group 3 00:14.0 USB controller [0c03]: Intel Corporation Device [8086:7ae0] (rev 11)
IOMMU Group 3 00:14.2 RAM memory [0500]: Intel Corporation Device [8086:7aa7] (rev 11)
IOMMU Group 4 00:15.0 Serial bus controller [0c80]: Intel Corporation Device [8086:7acc] (rev 11)
IOMMU Group 5 00:16.0 Communication controller [0780]: Intel Corporation Device [8086:7ae8] (rev 11)
IOMMU Group 6 00:17.0 SATA controller [0106]: Intel Corporation Device [8086:7ae2] (rev 11)
IOMMU Group 7 00:1f.0 ISA bridge [0601]: Intel Corporation Device [8086:7a86] (rev 11)
IOMMU Group 7 00:1f.3 Audio device [0403]: Intel Corporation Device [8086:7ad0] (rev 11)
IOMMU Group 7 00:1f.4 SMBus [0c05]: Intel Corporation Device [8086:7aa3] (rev 11)
IOMMU Group 7 00:1f.5 Serial bus controller [0c80]: Intel Corporation Device [8086:7aa4] (rev 11)
IOMMU Group 7 00:1f.6 Ethernet controller [0200]: Intel Corporation Ethernet Connection (17) I219-V [8086:1a1d] (rev 11)
IOMMU Group 8 01:00.0 VGA compatible controller [0300]: NVIDIA Corporation GK106 [GeForce GTX 660] [10de:11c0] (rev a1)
IOMMU Group 8 01:00.1 Audio device [0403]: NVIDIA Corporation GK106 HDMI Audio Controller [10de:0e0b] (rev a1)





nano /etc/default/grub

"""
GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo Debian`
- GRUB_CMDLINE_LINUX_DEFAULT="quiet"
+ GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on vfio_iommu_type1.allow_unsafe_interrupts=1 iommu=pt vfio-pci.ids=10de:11c0"
GRUB_CMDLINE_LINUX=""
"""
sudo update-grub

起動（コンソールダメ）
直ぐエンター小瀬！！

QEMU!!!!

