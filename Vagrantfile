
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  disks_dir = File.join(File.dirname(File.expand_path(__FILE__)), "disks")
  FileUtils.mkdir_p(disks_dir)

  config.vm.provider "virtualbox" do | vb |
    if Dir.empty?(disks_dir) 
      vb.customize ["storagectl", :id, "--name", "SATA Controller", "--add", "sata"]
    end

    for i in 1..4
      disk_name = "hdd_drive#{i}.vdi" % [i]
      disk_path = File.join(disks_dir, disk_name)
      if !File.exist?(disk_path)
        vb.customize ["createhd", "--filename", disk_path, "--size", 300]
      end
      vb.customize ["storageattach", :id, "--storagectl", "SATA Controller", "--port", i, "--device", 0, "--type", "hdd", "--medium", disk_path]
    end
  end

  config.vm.provision "shell", path: "script.sh"

end
