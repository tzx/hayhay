Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2004"
  config.vm.box_check_update = true

  LoadBalancerCount = 2
  (1..LoadBalancerCount).each do |i|
    config.vm.define "lb#{i}" do |lb|
      lb.vm.hostname = "lb#{i}"
      lb.vm.network "private_network", ip: "192.168.33.5#{i}"
      lb.vm.provider "libvirt" do |lv|
        lv.memory = 512
        lv.cpus = 1
      end
    end
  end

  ControllerCount = 3
  (1..ControllerCount).each do |i|
    config.vm.define "controller#{i}" do |controller|
      controller.vm.hostname = "controller#{i}"
      controller.vm.network "private_network", ip: "192.168.33.10#{i}"
      controller.vm.provider "libvirt" do |lv|
        lv.memory = 2048
        lv.cpus = 2
      end
      controller.vm.provision "ansible" do |ansible|
        ansible.playbook = "k8s-playbook.yml"
      end
    end
  end

  WorkerCount = 3
  (1..WorkerCount).each do |i|
    config.vm.define "worker#{i}" do |worker|
      worker.vm.hostname = "worker#{i}"
      worker.vm.network "private_network", ip: "192.168.33.20#{i}"
      worker.vm.provider "libvirt" do |lv|
        lv.memory = 2048
        lv.cpus = 1
        lv.storage :file, :size => '10G'
      end
      worker.vm.provision "ansible" do |ansible|
        ansible.playbook = "k8s-playbook.yml"
      end
    end
  end
end
