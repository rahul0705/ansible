Vagrant.configure("2") do |config|
  boxes = ["ubuntu/xenial64", "ubuntu/trusty64"]
  boxes.each_index do |machine_id|
    config.vm.define "machine#{machine_id}" do |machine|
      machine.vm.box = boxes[machine_id]
      machine.vm.hostname = "machine#{machine_id}"
      machine.vm.network "private_network", ip: "192.168.77.#{20+machine_id}"
      if machine.vm.box.include? "ubuntu"
        machine.vm.provision "shell", inline: "apt-get update; apt-get -y install python"
      end

      # Only execute once the Ansible provisioner,
      # when all the machines are up and ready.
      if machine_id == boxes.length - 1
        machine.vm.provision :ansible do |ansible|
          # Disable default limit to connect to all the machines
          ansible.limit = "all"
          ansible.playbook = "site.yml"
          ansible.become = true
          ansible.ask_vault_pass = true
          ansible.extra_vars = {
            initial_setup: true
          }
        end
      end
    end
  end
end
