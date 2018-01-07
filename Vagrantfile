# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/xenial64"
  config.vm.synced_folder "./docker", "/vagrant", :nfs => true

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", 1024]  # メモリ割り当て
    vb.customize ["setextradata", :id, "VBoxInternal/Devices/VMMDev/0/Config/GetHostTimeDisabled", 0] # Vagrant側とホスト(mac, widnwows等)の時刻同期
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end

  config.vm.provision "shell", inline: <<-SHELL
    # https経由でDLするために以下をインストール
    apt-get install -y apt-transport-https ca-certificates curl software-properties-common
    # 公式の手順に従う https://docs.docker.com/engine/installation/linux/docker-ce/debian/#set-up-the-repository
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    apt-key fingerprint 0EBFCD88
    # apt-getはリポジトリ登録したものしかダウンロードできないため以下を登録
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    apt-get update
    apt-get install -y docker-ce
    # 意図しないユーザーがコマンド打っちゃった!?っていうのを防ぐ
    groupadd docker
    usermod -aG docker $USER
    curl -L "https://github.com/docker/compose/releases/download/1.17.1/docker-compose-$(uname -s)-$(uname -m)" >  /usr/bin/docker-compose
    # /usr/bin/docker-composeの実行権限を与えている
    chmod +x /usr/bin/docker-compose
  SHELL

  config.vm.network "private_network", ip: "192.168.35.101"
end
