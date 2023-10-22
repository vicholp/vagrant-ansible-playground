ENV['VAGRANT_DEFAULT_PROVIDER'] = 'libvirt'

Vagrant.configure("2") do |config|
  N = 4

  (1..N).each do |instance|
    config.vm.define "vm#{instance}" do |machine|
      machine.vm.box = "generic/alpine318"

      machine.vm.network :private_network,
        :ip => "192.168.10.23#{instance}",
        :libvirt__domain_name => "vm#{instance}"
    
      machine.vm.provider :libvirt do |libvirt|
        libvirt.cpus = 2
        libvirt.memory = 3072
      end

      machine.vm.synced_folder "/home/vicho/dev/utfsm/Grupo01-Laboratorio-2", "/home/vagrant/Grupo01-Laboratorio-2", type: "rsync"

      if instance == N
        machine.vm.provision :ansible do |ansible|
          ansible.limit = "all"
          ansible.playbook = "playbook.yml"
          ansible.groups = {
            "vm" => ["vm[1:4]"],

            "vm:vars" => {
              "ansible_python_interpreter" => "/usr/bin/python3"
            },

            "oms" => ["vm1"],
            "onu" => ["vm2"],
            "datanode1" => ["vm3"],
            "datanode2" => ["vm4"],
            "continents" => ["vm[1:4]"],
          }
        end
      end
    end
  end
end
  
