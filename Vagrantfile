Vagrant.configure("2") do |config|
  # Use Ubuntu 16.04 as our base image
  config.vm.box = "ubuntu/xenial64"

  # Define a static IP for this machine
  # IMPORTANT: add this IP to your /etc/hosts file on your host OS
  config.vm.network "private_network", ip: "192.168.50.4"

  # Install python, an Ansible dependency
  config.vm.provision :shell, :inline => "sudo apt-get update && sudo apt-get install -y python"

  # Provision the machine with Ansible
  config.vm.provision :ansible do |ansible|
    ansible.inventory_path = "ansible_hosts"
    ansible.playbook = "playbook.yml"
    ansible.limit = "footprints_local"
  end
end
