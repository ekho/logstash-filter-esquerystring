# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
  end

  config.vm.synced_folder ".", "/elk/logstash-plugings/logstash-filter-esquerystring"

  config.vm.provision "shell", inline: <<-SHELL
    set -e
    apt-get update
    apt-get dist-upgrade -y
    apt-get install -y openjdk-8-jdk
    gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
    curl -sSL https://get.rvm.io | bash -s stable --ruby
    source /usr/local/rvm/scripts/rvm

    export LOGSTASH_BRANCH=6.x
    mkdir -p ~/.gradle && echo "org.gradle.daemon=false" >> ~/.gradle/gradle.properties

    rvm use jruby-9.1.13.0 --install --binary --fuzzy
    gem install bundler

    cd /elk/logstash-plugings/logstash-filter-esquerystring
    export BUNDLE_GEMFILE=$PWD/Gemfile
    ci/build.sh
  SHELL
end
