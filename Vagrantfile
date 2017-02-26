# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.network "forwarded_port", guest: 2480, host: 2480
  config.vm.network "forwarded_port", guest: 2424, host: 2424
  config.vm.synced_folder "shared", "/shared"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get -y install build-essential curl python-software-properties
    apt-get install -y default-jre
    wget https://orientdb.com/download.php?file=orientdb-community-2.1.3.tar.gz -O orientdb-community-2.1.3.tar.gz
    tar -xf orientdb-community-2.1.3.tar.gz
    mv orientdb-community-2.1.3 /opt/orientdb
    useradd -r orientdb -s /bin/false
    chown -R orientdb:orientdb /opt/orientdb
    cp /shared/console.sh /usr/bin/orientdb
    cp /shared/init.d/orientdb /etc/init.d/orientdb
    cp -f /shared/orientdb-server-config.xml /opt/orientdb/config
    cd /etc/init.d && update-rc.d orientdb defaults
    chown orientdb:orientdb /opt/orientdb/config/orientdb-server-config.xml
    service orientdb start
  SHELL
  
end
