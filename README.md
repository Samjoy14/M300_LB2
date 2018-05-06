# M300_LB2_Docker

Vagrantfile
-------------
Zuerst habe ich eine Docker Umgebung aufgebaut. Ein Beispiel File und der Code befindet sich <a href="https://github.com/mc-b/devops/tree/master/docker/dc">Hier</a>.
Deshalb musste ich eine Docker Standard Vagrantfile erstellt Aus diesem Grund habe ich ein Beispiel Vagrantfile genommen und dementsprechend modifiziert.

Für alle VMs wird die Ubuntu Box verwendet und hier noch den <a href="https://app.vagrantup.com/ubuntu/boxes/xenial64">Link</a>. 
Bei meinen Fall habe in einem Container zwei VMs erstellt. Hier noch die dazu gehörige Vagrantfile. 

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

#
#	Ubuntu Xenial 64-bit Linux mit Docker
#

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"
  
  # Docker Provisioner
  config.vm.provision "docker" do |d|
  end  

  # Create a public network, which generally matched to bridged network.
  config.vm.define "mysql" do |mysql|
	db.vm.provider "virtualbox" do |vb|
	  vb.memory = "1024"  
	end
    db.vm.hostname = "mysql"
    db.vm.network "private_network", ip: "192.168.100.100"
	db.vm.network "forwarded_port", guest:3376 , host:3376, auto_correct: false
	db.vm.network "forwarded_port", guest:443 , host:443, auto_correct: false
  end
  
  config.vm.define "wordpress" do |web|
    web.vm.hostname = "wordpress"
    web.vm.network "private_network", ip:"192.168.100.101"
	web.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
	web.vm.provider "virtualbox" do |vb|
	  vb.memory = "1024"  
	end     
  end 
   
end
```
MySql
-------------
Nach der Installation mit Vagrantfile. Kann man mit der Docker einrichten. Zuerst muss ich im Bash über vagrant ssh mysql verbinden. 
In diesem Verzeichnis muss ich nun folgende Befehle eingeben.

```
vagrant ssh mysql

docker images
docker ps -a
docker pull mysql
docker run --name qas-sql -e MYSQL_ROOT_PASSWORD=password -d mysql:latest

```
Nach diesem Befehlen ist mit der Msql Arbeit abgeschlossen. 

Wordpress
-------------
Nach der Mysql Schritte kann man nun mit der Wordpress arbeiten. Nun muss ich im Bash über vagrant ssh wordpress verbinden. 
Nun muss ich in diesem Verzeichnis die folgende Befehle eingeben.

```
vagrant ssh wordpress

docker images
docker ps -a
docker pull wordpress
docker run --name qas-wordpress --link qas-sql:mysql -p 8080:80 -d wordpress
```

Nach diesem Schritten will ich die Wordpress über Benutzer Oberfläche zugreifen. 
 Dementsprechend muss ich den IP Adresse von Wordpress wissen. Ich gebe nun folgenden Befehle ein.

```
vagrant ssh wordpress

docker-machine ip
"192.168.100.101"
```
Der IP Adresse heisst "192.168.100.101". Der Grund ist, weil es über die Vagrantfile definiert worden war.
Nun kann man Wordpress über die Benutzer Oberfläche zugreifen. Und zwar über unter [https://192.168.100.101](https://192.168.100.101) erreicht werden.

Alle Informationen, Angaben und Textbeiträge innerhalb sind ohne Gewähr auf Vollständigkeit und Richtigkeit.



