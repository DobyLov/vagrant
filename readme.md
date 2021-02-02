# Vagrant

Provisionner des machines rapidement pour réaliser des 
Pour déployer X machines types il suffit de modifier la variable cluster et de preciser les nombre de machines, nom, ip, cpu et ram en Mo :
**Example:** 
cluster = {
        "k8smaster0" => { :ip => "192.168.1.100", :cpus => 2, :mem => 3072 },
        #"k8smaster0" => { :ip => "192.168.1.101", :cpus => 2, :mem => 3072 },
        "k8sworker0" => { :ip => "192.168.1.102", :cpus => 2, :mem => 3072 },
        "k8sworker1" => { :ip => "192.168.1.103", :cpus => 2, :mem => 3072 },
        "k8sworker2" => { :ip => "192.168.1.104", :cpus => 2, :mem => 3072 }
}

**Boxes**
Os Centos: 

faire vagrant up
