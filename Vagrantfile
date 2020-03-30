nodes = [
  { :hostname => 'debian-A',   :ip => '192.168.31.111', :image => "debian/buster64", :ssh_port => '2221'},
  { :hostname => 'debian-B',   :ip => '192.168.31.112', :image => "debian/buster64", :ssh_port => '2222'},
  { :hostname => 'centos-C',   :ip => '192.168.31.113', :image => "centos/7", :ssh_port => '2223'},
  
]

Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 3
  end

  nodes.each do |node|
    config.vm.define node[:hostname] do |nodeconfig|
      nodeconfig.vm.hostname = node[:hostname]
      nodeconfig.vm.network :public_network, ip: node[:ip], bridge: "eno1"
      nodeconfig.vm.box = node[:image]
      nodeconfig.vm.network :forwarded_port, guest: '22', host: node[:ssh_port], id: "ssh"

      # if node[:type] == "elk"
        # nodeconfig.vm.network :forwarded_port, guest: 5601, host: node[:kibana], id: "kibana"
        # nodeconfig.vm.provision :file, source: "/home/rodce/Projects/elk/elasticsearch-7.6.1.tar", destination: "/home/vagrant/"
        # nodeconfig.vm.provision :file, source: "/home/rodce/Projects/elk/kibana-7.6.1-linux-x86_64.tar", destination: "/home/vagrant/"

        # nodeconfig.vm.provision :shell, inline: "tar -xf /home/vagrant/elasticsearch-7.6.1.tar"
        # nodeconfig.vm.provision :shell, inline: "tar -xf /home/vagrant/kibana-7.6.1-linux-x86_64.tar"

      # end
      
      # nodeconfig.vm.network :forwarded_port, guest: 22, host: node[:port], id: "ssh"

    end

  end
  config.vm.provision "ansible" do |ansible|
    ansible.inventory_path = "provisioning/hosts"
    ansible.playbook = "provisioning/playbook.yml"
    # ansible.groups = {
    #   "debin" => ["debian-A",'debian-B'],
    #   "centos" => ["centos_C"],
    # }  
      # "group4" => ["other_node-[a:d]"], # silly group definition
  end
end
