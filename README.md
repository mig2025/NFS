Vagrant стенд для NFS:
-----
Рабочее место: Windwos 11, Virtual Box 7.0.18, Visual Studio Code

Vagrantfile поднимает -> VM NFS-Server->192.168.56.10, VM NFS-Client->192.168.56.11 -> Ubuntu 22.04

Имя хоста: nfss - Server, nfsc - Client.

Настройка:
-----
Сервер:
-----

Замена команд на свои:

<< EOF > /etc/exports /srv/share 192.168.50.11/32(rw,sync,root_squash) EOF -> echo "/srv/share 192.168.56.11/(rw,sync,root_squash)" | tee -a /etc/exports

Клиент:
-----
Замена команд на свои:

echo "192.168.50.10:/srv/share/ /mnt nfs vers=3,noauto,x-systemd.automount 0 0" >> /etc/fstab ->

echo "192.168.56.10:/srv/share /mnt/share nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0" |tee -a /etc/fstab