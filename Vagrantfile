Vagrant.configure("2") do |config|
  # Jenkins Server VM
  config.vm.define "jenkins" do |jenkins|
    jenkins.vm.box = "ubuntu/jammy64"
    jenkins.vm.hostname = "jenkins-server"
    jenkins.vm.network "private_network", ip: "192.168.56.10"
    jenkins.vm.provider "virtualbox" do |vb|
      vb.name = "jenkins-server"
      vb.memory = "8048"
      vb.cpus = 2
    end
  end

  # Deployment Server VM
  config.vm.define "deploy" do |deploy|
    deploy.vm.box = "ubuntu/jammy64"
    deploy.vm.hostname = "deploy-server"
    deploy.vm.network "private_network", ip: "192.168.56.11"
    deploy.vm.provider "virtualbox" do |vb|
      vb.name = "deploy-server"
      vb.memory = "2024"
      vb.cpus = 1
    end
  end
end

