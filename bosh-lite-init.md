<pre>
1. ruby install
yum install -y git-core zlib zlib-devel gcc-c++ patch readline readline-devel libyaml-devel libffi-devel openssl-devel make bzip2 autoconf automake libtool bison curl sqlite-devel

cd
git clone git://github.com/sstephenson/rbenv.git .rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
exec $SHELL

git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bash_profile
exec $SHELL

sudo su -

. ./.bash_profile
rbenv install -v 2.2.1
rbenv global 2.2.1

ruby -v

2. bosh_cli install
gem install bosh_cli --no-ri --no-rdoc

3. virtual box install & reboot
yum install -y wget
wget https://releases.hashicorp.com/vagrant/1.9.3/vagrant_1.9.3_x86_64.rpm?_ga=1.214157015.1947354080.1492659306
mv vagrant_1.9.3_x86_64.rpm* vagrant_1.9.3_x86_64.rpm
rpm -ivh vagrant_1.9.3_x86_64.rpm

wget http://download.virtualbox.org/virtualbox/rpm/el/virtualbox.repo
mv virtualbox.repo /etc/yum.repos.d/
yum install -y VirtualBox-5.1
yum -y update
yum install -y kernel-devel

4. vm up
#vagrant plugin install vagrant-openstack-provider

#sudo /sbin/vboxconfig
vagrant init cloudfoundry/bosh-lite;

[root@kkw-bosh-lite ~]# cat Vagrantfile|grep -v "#"|grep -v "^$"
Vagrant.configure("2") do |config|
  config.vm.box = "cloudfoundry/bosh-lite"
   config.vm.provider "virtualbox" do |vb|
     vb.memory = "14336"
     vb.cpus = 6
   end
end

vagrant up --provider virtualbox

5. bosh lite & cf-release

cd ~
mkdir workspace
cd workspace
git clone https://github.com/cloudfoundry/bosh-lite
cd bosh-lite

export no_proxy=xip.io,192.168.50.4
bosh target 192.168.50.4 lite
~/workspace/bosh-lite/bin/add-route

cd ~/work*
git clone https://github.com/cloudfoundry/cf-release.git
cd cf-release
./scripts/update

yum install -y unzip
wget https://github.com/cloudfoundry-incubator/spiff/releases/download/v1.0.8/spiff_linux_amd64.zip
unzip spiff*.zip
mv spiff /usr/bin
gem install bundler

./scripts/generate-bosh-lite-dev-manifest
cd ~/workspace/bosh-lite
./bin/provision_cf
cd ~/workspace/cf-release
bosh deploy
</pre>
