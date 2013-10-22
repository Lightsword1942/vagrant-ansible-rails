# Rails dev box

Builds an Ubuntu Linux VM for Rails development using Ansible and Vagrant.

This box provides the core packages needed for typical Rails 4 apps and nothing else.

The ultimate goal here is to be able to boot up a new Ruby on Rails VM more or less instantly.

## Installs

+ Ruby 2
+ Postgres
+ Nginx

## Use

```
# Bring both the webserver and db server online
vagrant up
# Provision the VM
./deploy.sh

# SSH into your new VM
vagrant ssh web

# Everything inside /vagrant is shared with your local file system

cd /vagrant

# Create your app
sudo gem install --no-rdoc --no-ri rails
sudo rails new myapp --database postgresql
cd myapp
# Create and migrate the database
bundle exec rake db:create && rake db:migrate
```

## Configure Postgres

Since postgres is running on a separate server we need to do some groundwork

```  
vagrant ssh db
sudo -u postgres psql
postgres=#CREATE USER vagrant WITH SUPERUSER PASSWORD 'vagrant';

# You can now log in to postgres using the username vagrant and password vagrant
psql -h 192.168.111.223 -U vagrant -d mydb
````

## Workflow

The idea is you work and edit your files locally but run the app inside your VM. 

All the files in your current directory (where your vagrant file is)
can be viewed in your VM in /vagrant once you SSH in to the box.

## Misc

Testing your SSH connection

```
ansible -m ping all -i devops/hosts -u vagrant --private-key=~/.vagrant.d/insecure_private_key

```

