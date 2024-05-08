Tässä on lyhyt kuvaus sisällöstä, jolla voidaan määrittää VM:lle haluttu määrä uusia koneita Vagrantilla.
Kyseinen tunniste tulee syöttää Vagrantfilen sisälle.

# -*- mode: ruby -*-
# vi: set ft=ruby :
# Copyright 2014-2023 Tero Karvinen http://TeroKarvinen.com

$minion = <<MINION
sudo apt-get update
sudo apt-get -qy install salt-minion
echo "master: 192.168.12.3">/etc/salt/minion
sudo service salt-minion restart
echo "See also: https://terokarvinen.com/2023/salt-vagrant/"
MINION

$master = <<MASTER
sudo apt-get update
sudo apt-get -qy install salt-master
echo "See also: https://terokarvinen.com/2023/salt-vagrant/"
MASTER

Vagrant.configure("2") do |config|
	config.vm.box = "debian/bullseye64"

	config.vm.define "t001" do |t001|
		t001.vm.provision :shell, inline: $minion
		t001.vm.network "private_network", ip: "192.168.12.100"
		t001.vm.hostname = "t001"
	end

	config.vm.define "t002" do |t002|
		t002.vm.provision :shell, inline: $minion
		t002.vm.network "private_network", ip: "192.168.12.102"
		t002.vm.hostname = "t002"
	end

	config.vm.define "tmaster", primary: true do |tmaster|
		tmaster.vm.provision :shell, inline: $master
		tmaster.vm.network "private_network", ip: "192.168.12.3"
		tmaster.vm.hostname = "tmaster"
	end
end

Vagrantin perustuksen lyhyt kuvaus alla

    cd C:\Users\Omakäyttäjä
    mkdir C:/Users/Omakäyttäjä/Kansio_johon_vagrantfile_halutaan
    cd C:\Users\Omakäyttäjä\Kansio_johon_Vagrantfile_halutaan
    vagrant init debian/bullseye64 -> perustaa kansioon Vagrantfilen
    notepad/nano/micro Vagrantfile -> Lisää uudet koneet tiedostoon -> tallenna muutokset
    vagrant up
    vagrant ssh haluttu_kone_sen_Vagrantfilessä_muodostetulla_nimellä -> vagrant ssh t001


