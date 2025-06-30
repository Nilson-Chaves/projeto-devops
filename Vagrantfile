Vagrant.configure("2") do |config|
  config.vm.box = "bento/debian-12"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.ssh.insert_key = false

  config.vm.provider "virtualbox" do |vb|
    vb.linked_clone = true
    vb.check_guest_additions = false
  end

  config.vm.define "arq" do |arq|
    arq.vm.hostname = "arq.nilson.wellington.devops"
    arq.vm.network "private_network", ip: "192.168.56.102"
    arq.vm.provision "shell", inline: "true"
    arq.vm.provider "virtualbox" do |vb|
      vb.memory = 512
      vb.customize ["createhd", "--filename", "disk1.vdi", "--size", 10240]
      vb.customize ["createhd", "--filename", "disk2.vdi", "--size", 10240]
      vb.customize ["createhd", "--filename", "disk3.vdi", "--size", 10240]
      vb.customize ["storageattach", :id, "--storagectl", "SATA Controller", "--port", 1, "--device", 0, "--type", "hdd", "--medium", "disk1.vdi"]
      vb.customize ["storageattach", :id, "--storagectl", "SATA Controller", "--port", 2, "--device", 0, "--type", "hdd", "--medium", "disk2.vdi"]
      vb.customize ["storageattach", :id, "--storagectl", "SATA Controller", "--port", 3, "--device", 0, "--type", "hdd", "--medium", "disk3.vdi"]
    end
  end

  ["db", "app", "cli"].each do |name|
    config.vm.define name do |node|
      node.vm.hostname = "#{name}.nilson.wellington.devops"
      node.vm.network "private_network", type: "dhcp"
      node.vm.provider "virtualbox" do |vb|
        vb.memory = (name == "cli" ? 1024 : 512)
      end
      node.vm.provision "shell", inline: "true"
    end
  end
end

