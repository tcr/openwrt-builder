# -*- mode: ruby -*-
# vi: set ft=ruby :

# ip addr prefix
addr_prefix="172.23.23"
image_name = "bin/x86/openwrt-x86-generic-combined-ext4.img.gz"
path = File.dirname __FILE__

Vagrant.configure("2") do |config|

  config.vm.define :builder do |config|
    config.vm.box = "openwrt-builder"
    config.vm.host_name = "openwrt-builder"
    config.vm.box_url = "http://puppetlabs.s3.amazonaws.com/pub/squeeze64.box"
    config.vm.network :private_network, ip: "#{addr_prefix}.10"
    config.vm.provider :virtualbox do |vbox|
      vbox.customize ["modifyvm", :id, "--memory", "1024"]
    end
    config.vm.synced_folder "./bin", "/mnt/images"
    #config.vm.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/build", "1"] # See https://www.virtualbox.org/ticket/10085
    if not (RUBY_PLATFORM.downcase.include?("mswin") or RUBY_PLATFORM.downcase.include?("mingw") )
      config.vm.provider :virtualbox do |vbox|
        vbox.customize ["modifyvm", :id, "--cpus",
          `awk "/^processor/ {++n} END {print n}" /proc/cpuinfo 2> /dev/null || sh -c 'sysctl hw.logicalcpu 2> /dev/null || echo ": 2"' | awk \'{print \$2}\' `.chomp ]
      end
    end
    config.vm.provision :shell, :inline => "apt-get -y update && curl -sSL https://get.rvm.io | bash -s stable --ruby"
    config.vm.provision :shell, :inline => "chef-solo --help 2>/dev/null 1>/dev/null || (apt-get -y install rubygems && gem install chef)"
    config.vm.provision :chef_solo do |chef|
      chef.cookbooks_path = "./chef/cookbooks"
      chef.roles_path = "./chef/roles"
      chef.data_bags_path = "./chef/data_bags"
      chef.add_role "builder"
    end
  end

#  config.vm.define :openwrt do |config|
#    config.vm.box = "openwrt-guest-#{File.stat(image_name).ctime.to_i}"
#    config.vm.host_name = "openwrt-guest"
#    config.vm.box_url = "#{path}/#{image_name}"
#    config.vm.network :hostonly, "#{addr_prefix}.11"
#    config.vm.boot_mode = :gui
#  end

  # Install VBGuest plugin
  # vagrant gem install vagrant-vbguest
  # do NOT download the iso file from a webserver
  # config.vbguest.no_remote = true

end
