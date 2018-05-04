# Plugin checker by DevNIX: https://github.com/DevNIX/Vagrant-dependency-manager
# vagrant-reload is required to solve issues with rebooting Windows machines after domain join
#require File.dirname(__FILE__)+'./Vagrant/Plugin/dependency_manager'
#check_plugins ['vagrant-reload']

# Variables
# Domain / Network
net_prefix            = '192.168.56'
box_name              = 'adamrushuk/win2016-std-dev'
box_version           = '0.2.1'
dc01_ip               = "#{net_prefix}.200"
domain_controller     = 'dc01'
domain_name           = 'lab.local'
netbios_name          = 'LAB'
safemode_admin_pw     = 'Passw0rds123'
domain_admins         = 'vagrant'

Vagrant.configure('2') do |config|
  # Box
  config.vm.box         = box_name
  config.vm.box_version = box_version

  # VirtualBox global box settings
  config.vm.provider 'virtualbox' do |vb|
    vb.linked_clone = true
    vb.gui          = true
    vb.customize ['modifyvm', :id, '--clipboard', 'bidirectional']
    vb.customize ['setextradata', 'global', 'GUI/SuppressMessages', 'all']
  end

  # WinRM plaintext is required for the domain to build properly (These settings should NOT be used on production machines)
  config.vm.communicator       = 'winrm'
  config.winrm.transport       = :plaintext
  config.winrm.basic_auth_only = true

  # Increase timeout in case VMs joining the domain take a while to boot
  config.vm.boot_timeout = 600

  # DC
  config.vm.define 'dc01' do |dc01|
    # CPU and RAM
    dc01.vm.provider 'virtualbox' do |vb|
      vb.cpus = '2'
      vb.memory = '2048'
    end

    # Hostname and networking
    dc01.vm.hostname = 'dc01'
    dc01.vm.network 'private_network', ip: dc01_ip
    dc01.vm.network 'forwarded_port', guest: 3389, host: 40200, auto_correct: true
  end

end
