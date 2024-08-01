# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.define "nfss" do |nfss|
    nfss.vm.box  = "ubuntu/jammy64"
    nfss.vm.network "private_network", ip: "192.168.56.10"
    nfss.vm.hostname = "nfss"
    nfss.vm.provision "shell" , inline: <<-SHELL
      apt-get update
      apt-get install -y nfs-kernel-server
      systemctl enable nfs-server
      mkdir -p /srv/share/upload
      chown -R nobody:nogroup /srv/share
      chmod 0777 /srv/share/upload
      echo "/srv/share 192.168.56.11/(rw,sync,root_squash)" | tee -a /etc/exports
      exportfs -a
      apt install krb5-kdc krb5-admin-server
    SHELL
    nfss.vm.provider :virtualbox do |vb|
      vb.memory = "1024"
      vb.cpus = 1
    end
  end
config.vm.define "nfsc" do |nfsc|
  nfsc.vm.box  = "ubuntu/jammy64"
  nfsc.vm.network "private_network", ip: "192.168.56.11"
  nfsc.vm.hostname = "nfsc"
  nfsc.vm.provision "shell", inline: <<-SHELL 
    apt update
    apt install -y nfs-common
    mkdir -p /mnt/share
    echo "192.168.56.10:/srv/share    /mnt/share   nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0" |tee -a /etc/fstab
    mount -a
  SHELL
  nfsc.vm.provider :virtualbox do |vb|
    vb.memory = "1024"
    vb.cpus = 1
  end
end
end