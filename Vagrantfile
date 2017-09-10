# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility).

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"

  # config.vm.box_check_update = false

  config.vm.define "db" do |db|
    config.vm.network "forwarded_port", guest: 5432, host: 5432
    config.vm.network "private_network", ip: "192.168.1.11"
  end

  # config.vm.network "public_network"

  # config.vm.synced_folder "../data", "/vagrant_data"

  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = false
    vb.name = "dj-database"
  
    # Customize the amount of memory on the VM:
    vb.memory = "512"
  end

  config.vm.provision "shell", inline: <<-SHELL
      apt-get install -y postgresql
      postgres -c "psql -f /vagrant/db-config.sql"
      postgres -c 'echo "host all all 192.168.1.10/24 trust" >> /etc/postgresql/9.3/main/pg_hba.conf'
      postgres -c "echo listen_addresses=\\'*\\' >> /etc/postgresql/9.3/main/postgresql.conf"
      service postgresql restart
    SHELL
  end

  #

  config.vm.define "web" do |db|
    config.vm.network "forwarded_port", guest: 8000, host: 8000
    config.vm.network "private_network", ip: "192.168.1.10"
  end

  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = false
    vb.name = "dj"
  
    # Customize the amount of memory on the VM:
    vb.memory = "512"
  end

  config.vm.provision "shell", inline: <<-SHELL
      apt-get install -y python-pip python-dev libpq-dev postgresql postgresql-contrib
      pip install django flake8 psycopg2
    SHELL
  end

end