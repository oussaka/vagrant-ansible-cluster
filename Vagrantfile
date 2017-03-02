# -*- mode: ruby -*-
# vi: set ft=ruby :

# Specify Vagrant version and Vagrant API version
Vagrant.require_version '>= 1.8.0'
VAGRANTFILE_API_VERSION = '2'

# Read YAML file with VM details (box, CPU, and RAM)
machines = YAML.load_file(File.join(File.dirname(__FILE__), 'machines.yml'))

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  config.vm.box_check_update = true

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Always use Vagrant's default insecure key
  config.ssh.insert_key = false

  # Iterate through entries in YAML file to create VMs
  machines.each do |machine|
    config.vm.define machine['name'] do |srv|

      # Don't check for box updates
      # srv.vm.box_check_update = false
      srv.vm.hostname = machine['name']
      srv.vm.network :private_network, ip: machine['ip']

      # Disable default synced folder
      # srv.vm.synced_folder '.', '/vagrant', disabled: true
      srv.vm.synced_folder '.', '/vagrant', :mount_options => ["fmode=666"]

      # Configure the VM with RAM and CPUs per machines.yml (VirtualBox)
      srv.vm.provider 'virtualbox' do |vb, override|
        vb.memory = machine['ram']
        vb.cpus = machine['vcpu']
        override.vm.box = machine['vb_box']
      end # srv.vm.provider virtualbox

    end # config.vm.define
  end # machines.each

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook        = "ansible/playbook/main.yml"
    ansible.verbose         = true
    # ansible.raw_arguments   = " -vvvv"
    # ansible.install         = true
    # ansible.limit           = "all"
    # ansible.inventory_path  = "inventory"
    # ansible.limit         = ['localhost']
    # ansible.sudo           = true
  end

end # Vagrant.configure