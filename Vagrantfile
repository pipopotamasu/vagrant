# -*- mode: ruby -*-
# vi: set ft=ruby :
# hoge
# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "centos-7.2"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"
  # rail s用
  config.vm.network :"forwarded_port", guest: 3000, host: 3000

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"
  config.vm.synced_folder ".", "/vagrant", type: "nfs"
  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
  # git
  sudo yum -y install git

  # httpd
  sudo yum -y install httpd
  sudo service httpd start
  sudo chkconfig httpd on

  # MySQL
  ## MySQLと競合の可能性があるためmariaDBを削除
  sudo yum -y remove mariadb-libs
  sudo rm -rf /var/lib/mysql/
  ## yumレポジトリを追加
  sudo yum -y localinstall http://dev.mysql.com/get/mysql57-community-release-el7-7.noarch.rpm
  ## MySQL install
  sudo yum -y install mysql-community-server
  ## MySQLの自動起動
  sudo systemctl enable mysqld.service

  # Node
  sudo yum -y install epel-release
  sudo yum -y install nodejs

  # NVM
  git clone git://github.com/creationix/nvm.git ~/.nvm
  source ~/.nvm/nvm.sh

  # Ruby 2.4.1
  sudo yum -y install gcc zlib-devel openssl-devel sqlite sqlite-devel mysql-devel readline-devel libffi-devel
  cd /usr/local/src
  sudo wget http://cache.ruby-lang.org/pub/ruby/2.4/ruby-2.4.1.tar.gz
  sudo tar zxvf ruby-2.4.1.tar.gz
  cd ruby-2.4.1
  sudo ./configure
  sudo make
  sudo make install
  sudo ln -s /usr/local/bin/ruby /usr/bin/ruby

  # rbenv Rubyのバージョン切り替えができるツール
  cd
  git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
  ## zshの場合は.bash_profileではない場所にする
  echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
  echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
  exec $SHELL -l

  # ruby-build rbenvを便利に使うツールっぽい https://tsuchikazu.net/linux_ruby_on_rails_install/
  git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build

  # /usr/local以下のあ全ファイルに777の権限を与える(あんま良くないけどめんどいからこれでよしとしておく)
  sudo chmod 777 -R /usr/local/

  # rail 5.1.1をinstall
  gem install rails -v 5.1.1

  SHELL
end
