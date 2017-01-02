Vagrant.configure("2") do |config|
  # Morpheus yells: MACHINES!
  $nodes = {
    "Arithmetic Producer" => {
      "tag" => "arithproducer",
      "IP" => "192.168.50.4",
      "synced_folder_disabled" => true
    },
    "REST API" => {
      "tag" => "restapi",
      "IP" => "192.168.50.3",
      "synced_folder_disabled" => true
    }
  }
  # Default VM spec
  config.vm.box = "bento/centos-7.2"
  config.ssh.forward_agent = true
  config.vm.provider "virtualbox" do |vb|
    vb.customize [
      "modifyvm", :id,
      "--memory", "1024",
      "--cpus", "1",
      "--ioapic", "on",
      "--pae", "on",
      "--hwvirtex", "on",
      "--vtxvpid", "on",
      "--vtxux", "on",
      "--nestedpaging", "off"
    ]
  end

  # Boot them'all (nodes)
  $nodes.each do |key, value|
    tag = value["tag"]
    ip = value["IP"]
    syncf = value["synced_folder_disabled"]
    config.vm.define tag do |t|
      t.vm.hostname = tag
      t.vm.network "private_network", ip: ip
      t.vm.synced_folder '.', '/vagrant', disabled: syncf
      # Exiting notes
      t.vm.provision "shell", privileged: false, inline: <<-EOF
        echo "#{key} running on local server address http://#{ip}"
      EOF
    end
  end
  # Controller up!
  config.vm.define "controller" do |controller|
    controller.vm.hostname = "controller"
    controller.vm.network "private_network", ip: "192.168.50.2"
    controller.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "512", "--cpus", "1"]
    end
    # Execute provision playbook through Ansible instance installed on your local
    # enviroment to set up Ansible on controller node
    controller.vm.provision "ansible" do |ansible|
      ansible.playbook = "provision/playbooks/provision.yml"
    end
  end
end

