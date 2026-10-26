
# -*- mode: ruby -*-
# vi: set ft=ruby :

############################
# Definició de variables
############################

IMATGE_BOX = "debian/bookworm64"
GESTORMV = "virtualbox"

NOM_NODE = "pj9f4a7"
NOM_BASE = "pj9f4a7"
ID = "-jaroce"
NOM_DOMINI = ".fjeclot.local"

MEMORIA_RAM = 2048
NUM_CPUS = 2

PORT1_GUEST = 80
PORT1_HOST = 8000

PORT2_GUEST = 443
PORT2_HOST = 8443

PORT3_GUEST = 3306
PORT3_HOST = 13306

PROT = "tcp"

CARPETA_HOST = "../a7/codis"
CARPETA_GUEST = "/home/vagrant/codis"

LLISTAT_PROGRAMARI_BASIC = "net-tools whois aptitude git zip unzip openssh-server"
LLISTAT_PROGRAMARI_DEPENDENCIES_DOCKER = "ca-certificates curl gnupg"
LLISTAT_PROGRAMARI_DOCKER = "docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin"

USUARI_VAGRANT = "vagrant"
GRUPS_USUARI_VAGRANT = "docker"

############################
# Creació de la màquina virtual
############################

Vagrant.configure("2") do |maquines|

  maquines.vm.define NOM_NODE + ID do |maquina|

    maquina.vm.provider GESTORMV do |parametres|
      parametres.name = NOM_NODE + ID
      parametres.memory = MEMORIA_RAM
      parametres.cpus = NUM_CPUS
      parametres.customize [
        "modifyvm", :id,
        "--clipboard", "bidirectional",
        "--groups", "/ASIX2_PJ9"
      ]
    end

    maquina.vm.box = IMATGE_BOX
    maquina.vm.hostname = NOM_BASE + ID + NOM_DOMINI

    maquina.vm.network "forwarded_port",
      guest: PORT1_GUEST, host: PORT1_HOST, protocol: PROT

    maquina.vm.network "forwarded_port",
      guest: PORT2_GUEST, host: PORT2_HOST, protocol: PROT

    maquina.vm.network "forwarded_port",
      guest: PORT3_GUEST, host: PORT3_HOST, protocol: PROT

    maquina.vm.synced_folder CARPETA_HOST, CARPETA_GUEST

    maquina.vm.provision "shell", inline: <<-SHELL
      apt-get update -y

      apt-get install -y #{LLISTAT_PROGRAMARI_BASIC}
      apt-get install -y #{LLISTAT_PROGRAMARI_DEPENDENCIES_DOCKER}

      install -m 0755 -d /etc/apt/keyrings

      curl -fsSL https://download.docker.com/linux/debian/gpg \
        -o /etc/apt/keyrings/docker.asc

      chmod a+r /etc/apt/keyrings/docker.asc

      echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian bookworm stable" \
        > /etc/apt/sources.list.d/docker.list

      apt-get update -y

      apt-get install -y #{LLISTAT_PROGRAMARI_DOCKER}

      usermod -aG #{GRUPS_USUARI_VAGRANT} #{USUARI_VAGRANT}
    SHELL

  end
end
