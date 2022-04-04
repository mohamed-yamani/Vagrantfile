MASTER_NAME = "myamani"
MASTER_IP = "192.168.42.110"
# mv /Users/myamani/.vagrant.d /goinfre/myamani && ln -s /goinfre/myamani/.vagrant.d /Users/myamani/.vagrant.d
# docker system prune
MASTER_SCRIPT = <<-SHELL
apt-get update
apt-get install -y -q net-tools curl vim make
curl -fsSL https://get.docker.com/ -o get-docker.sh
sh get-docker.sh
    systemctl start docker
    systemctl enable docker
    curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose
    docker ps && echo "[INFO] Docker installed successful" || echo"[INFO] Error installing Docker"
    rm -rf get-docker.sh
    usermod -aG docker vagrant
    echo "127.0.0.1       myamani.42.fr" >> /etc/hosts
    export HOME=/vagrant
    # cd /vagrant && docker-compose down && docker-compose up -d --build && docker container ps
SHELL


MEM = 2048
CPU = 2

Vagrant.configure("2") do |config|
    config.vm.box = "debian/buster64"
    config.vm.provider "virtualbox" do |vb|
        vb.memory = MEM
        vb.cpus = CPU
    end

    config.vm.define MASTER_NAME do |control|
        control.vm.hostname = MASTER_NAME
        control.vm.network "private_network", ip: MASTER_IP
        control.vm.provider "virtualbox" do |vb|
            vb.name = MASTER_NAME
        end
        control.vm.provision "shell", inline: MASTER_SCRIPT
    end
end