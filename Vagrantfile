# -*- mode: ruby -*-
# vi: set ft=ruby :

# Clean environment for running tests locally

Vagrant.configure("2") do |config|

    ruby_version = "2.2.0"
    puppet_version = "4.6.1"
    hostname = "jerakia.box"
    locale = "en_GB.UTF.8"

    # Box
    config.vm.box = "ubuntu/trusty64"

    # SSH
    config.ssh.insert_key = false

    # Shared folders
    config.vm.synced_folder ".", "/srv"

    # Setup
    config.vm.provision :shell, :inline => "touch .hushlogin"
    config.vm.provision :shell, :inline => "hostnamectl set-hostname #{hostname} && locale-gen #{locale}"
    config.vm.provision :shell, :inline => "apt-get update --fix-missing"
    config.vm.provision :shell, :inline => "apt-get purge -q -y puppet puppet-common"
    config.vm.provision :shell, :inline => "apt-get install -q -y g++ make git curl vim"

    # RVM
    config.vm.provision :shell, :privileged => false, :inline => "gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3"
    config.vm.provision :shell, :privileged => false, :inline => "curl -L https://get.rvm.io | bash -s stable"
    config.vm.provision :shell, :privileged => false, :inline => "rvm install #{ruby_version} --autolibs=enabled && rvm --fuzzy alias create default #{ruby_version}"

    config.vm.provision :shell, :privileged => false, :inline => "echo \"export PUPPET_GEM_VERSION=#{puppet_version}\" >> ~/.profile"
    config.vm.provision :shell, :privileged => false, :env => { "BUNDLE_GEMFILE" => "/srv/Gemfile", "PUPPET_GEM_VERSION" => puppet_version }, :inline => " bundle install --jobs=3 --retry=3"
    config.vm.provision :shell, :privileged => false, :inline => "pushd /srv; bundle exec rake; popd"

end
