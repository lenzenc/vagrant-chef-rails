# Vargent-Chef-Rails
This is a very simple rails application that shows how to do development given a project like this using [Vagrant](https://www.vagrantup.com/) & [Chef](https://www.getchef.com/chef/).

## Setting Up Vagrant
Before you get started with this example application you will need to install the following on our local box;

* [Install Vagrant](http://www.vagrantup.com/downloads.html)
* [Install VirtualBox](https://www.virtualbox.org/wiki/Downloads)

VirtualBox is where your virtual machines will run. They will be headless which means that they will run in the background and you will interact with them over SSH.

Next we're going to install two plugins for Vagrant.

* vagrant-vbguest - automatically installs the host's VirtualBox Guest Additions on the guest system.
* vagrant-librarian-chef - let's us automatically run chef when we fire up our machine.
* vagrant-omnibus - a Vagrant plugin that ensures the desired version of Chef is installed via the platform-specific Omnibus packages.

Install Commands;

	vagrant plugin install vagrant-vbguest
	vagrant plugin install vagrant-librarian-chef
	vagrant plugin install vagrant-omnibus

## Using Rails inside Vagrant
After getting Vagrant setup on your local box you can now start using Vagrant to do development on this rails application.

### To Start

	git clone https://github.com/lenzenc/vagrant-chef-rails.git

__Vagrant Up__

	vagrant up

Vagrant Up will initially downloand a *blank* image of Ubuntu and then create a new VM and then use Chef to provision the resources that this sample application needs, i.e. ruby, rails, git, mysql etc.

Once *vagrant up* is complete you can now ssh into your newly created/started VM;

	vagrant ssh

Vagrant sets up the /vagrant folder as a shared directory between the virtual machine and your host operating system. If you cd /vagrant and run ls you will see all the files from this Rails application.

#### Initial Vagrant Up
If this is the first time you have run *vagrant up* or you previously ran *vagrant destroy* then you will want to run the following commands to initialize this Rails application.

	cd /vagrant
	bundle install
	rake db:create
	rake db:migrate

#### Future Vagrant Up
If you have already run *vagrant up* initially and have just updated this Rails application using git, i.e. *git pull* then you can just run the following command to make sure this Rails application is up to date.

	cd /vagrant
	rake db:migrate

#### Running this Rails Application
To run this example Rails application run the following command(s)

	cd /vagrant
	rails s

You will notice that this application is now running on localhost:3000, point your local box's browser to;

	http://localhost:3000

If all went well you should see a HTML page showing a H2 title of "Customer List".

You might be asking...__"how/why localhost"__, I thought this application was running on the VM"?  

Correct, but the cool thing about Vagrant is that given the Vagrant file that is at the rool of this project which describes the environment needed by this sample application it also sets up port forwarding on your local box to the VM...which is done by this line in the Vagrantfile;

	config.vm.network :forwarded_port, guest: 3000, host: 3000

### Development
Now that everything is setup and running you can modify files for this sample application on your local box and changes will automatically be picked up by the VM because the files on your local box are sync'ed to the */vagrant* directory on the VM.

## Other Vagrant Commands

* vagrant destroy - this will destroy your provisioned VM, next time you run *vagrant up* it will take the empty VM image and re-provision.
* vagrant halt - this will stop the running VM.
* vagrant box list - this will show all of the empty VM images that are available on your local box.
