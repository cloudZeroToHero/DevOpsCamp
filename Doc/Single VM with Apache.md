# How I created custom box with Apache

As a start point I have an Virtual Box VM created and prepared for Vagrant - [this is how i did it](https://github.com/cloudZeroToHero/DevOpsCamp/blob/main/Vagrant/Doc/Parent%20VM%20and%20Base%20Box.md)
It is of course possible to user one of the publicly available boxes from > https://app.vagrantup.com/boxes/search


First - I need a vagrantfile, so I run
```
vagrant init myUbuntu2204
```
or (with use of HashiCorp image)
```
vagrant init hashicorp/precise64
```


Of course the file will contain all the comments, but the simplified version is like this:
```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # myUbuntu2204 is custom box - please see "https://github.com/cloudZeroToHero/DevOpsCamp/blob/main/Vagrant/Doc/Parent%20VM%20and%20Base%20Box.md"
  config.vm.box = "myUbuntu2204"
  config.vm.box_check_update = false

  # uncomment following line if vagrant cannot login using SSH
  #config.ssh.username = "vagrant"
  #config.ssh.password = "vagrant"
  
  # Just to check if shell provisioning works I install Apache
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y apache2
  SHELL

end
```

To create VM from this file I used:

```
vagrant up --provider virtualbox
```
If you did not mess with VMware provider - like I did ( https://github.com/cloudZeroToHero/DevOpsCamp/blob/main/Vagrant/Doc/Add%20VMware%20provider.md ) you probably don't have to specify --provider option


When Vagrant will finish all the actions, I opened my yor favorite web browser pointing 

> 127.0.0.1:8080

to see the Apache2 Default Page 

Great success :wink:

