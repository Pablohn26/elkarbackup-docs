Debian batean oinarritutako instalazioa duen RAW irudia deskargatu eta zure birtualizazio sisteman gehitzea: Aukera desberdinak daude, guk proposatzen ditugunak Proxmox eta KVM dira, baina gehiago ere badaude: XEN, VMWare, eta abar. Dokumentu honen bukaeran dagoen “***Irudia beste birtualizazio sistema batzuetara bihurtzen***” atalean RAW irudia VMWare eta VirtualBox birtualizazio sistemetarako nola bihurtu daitekeen azaltzen dugu.

Azkeneko Irudiak hemendik deskargatu daitezke:
- http://ftp.tknika.net/elkarbackup/ElkarBackupServerBase2GB1.0.9_64b.img 64 biteko raw irudia KVM/Proxmox sistemetan erabiltzeko: 2,1Gb
- http://ftp.tknika.net/elkarbackup/ElkarBackupServerBase2GB1.0.9_64b.vmdk  64 biteko VMDK irudia VMWare sistemetan erabiltzeko: 1,4Gb

Irudiak instalazioa egina duenez lanean hasteko martxan jartzea besterik ez dago. Hala ere, eta gure sarera egokitzeko pare bat gauza aldatu beharko ditugu.

Zerbitzariak DHCP bidez jasotzen du bere helbidea, eta hau aldatu beharra daukagu beti helbide berdina izatea interesatzen zaigulako. IP finkoa jartzeko `/etc/network/interfaces` fitxategia editatuko dugu

```bash
root@ElkarBackup:~# nano /etc/network/interfaces
```


Eta hor Ipa, maskara, gateway eta DNSak jarriko ditugu.
```bash
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

# The loopback network interface
auto lo
iface lo inet loopback
# The primary network interface
allow-hotplug eth0
# iface eth0 inet dhcp
iface eth0 inet static
address NIREIP
netmask MASKARA
gateway GATEWAY
dns-nameservers DNSak

```


Seguruenez arrankatzen duenean ez du sare txartela aktibatuko, bere txartela (kasu honetan birtuala) aldatu dela konturatzen delako. ***70-persistent-net*** rules fitxategia editatu eta bere edukia ezabatu. Gero berrabiarazi ondoren hau berak zuzenduko du sare txartel birtualaren informazio egokia jarriz.
```bash
root@ElkarBackup:~# nano /etc/udev/rules.d/70-persistent-net.rules

```

Makina birtualean ***root*** erabiltzaileak ***root*** pasahitza du. Hori ere produkzioko zerbitzarian aldatu beharko litzateke.

```bash
root@ElkarBackup:~# passwd root
Introduzca la nueva contraseña de UNIX:
Vuelva a escribir la nueva contraseña de UNIX:
passwd: contraseña actualizada correctamente

```

Aldaketa hauek  egin ondoren makina birtuala berrabiarazi.
```bash
root@ElkarBackup:~# shutdown -r now

```
