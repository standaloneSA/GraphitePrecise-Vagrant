# -*- mode: ruby -*-
# vi: set ft=ruby :
#
# This vagrant box ends up being a little bit similar to Warwick Poole's here:
# http://warwickp.com/2012/03/vagrant-box-with-graphite-statsd-gdash-nginx
# but but I'm using Apache, and I'm building it on the fly in this file, rather
# than providing a binary image. I don't think it's groundbreaking, but it might
# help some people figure out what to do. 


#########################
$initScript = <<ENDSCRIPT

# Lets start out with the newest stuff. 
apt-get update
apt-get upgrade 


# General instructions for installing Graphite are from these pages: 
# http://graphite.wikidot.com/installation
# https://www.digitalocean.com/community/articles/installing-and-configuring-graphite-and-statsd-on-an-ubuntu-12-04-vps
# https://gist.github.com/jgeurts/3112065#file-install-graphite-ubuntu-12-04-sh-L41

apt-get install --assume-yes \
	apache2 \
	apache2-mpm-worker \
	apache2-utils \
	apache2.2-bin \
	apache2.2-common \
	libapr1 \
	libaprutil1 \
	libaprutil1-dbd-sqlite3 \
	build-essential \
 	python3.2 \
	python-dev \
	libpython3.2 \
	python3-minimal \
	libapache2-mod-wsgi \
 	libaprutil1-ldap \
	memcached \
	python-cairo-dev \
	python-django \
	python-django-tagging \
	python-ldap \
	python-memcache \
 	python-pysqlite2 \
	python-setuptools \
	python-zope.interface \
	python-twisted \
	python-twisted-bin \
	python-twisted-web \
	python-txamqp \
	sqlite3 \
	erlang-os-mon \
	erlang-snmp \
	rabbitmq-server \
	bzr \
	expect \
 	libapache2-mod-python \
	git \
	build-essential \
	ssh \
	python-setuptools


wget https://launchpad.net/graphite/0.9/0.9.10/+download/graphite-web-0.9.10.tar.gz
wget https://launchpad.net/graphite/0.9/0.9.10/+download/carbon-0.9.10.tar.gz
wget https://launchpad.net/graphite/0.9/0.9.10/+download/whisper-0.9.10.tar.gz
tar -zxvf graphite-web-0.9.10.tar.gz
tar -zxvf carbon-0.9.10.tar.gz
tar -zxvf whisper-0.9.10.tar.gz
mv graphite-web-0.9.10 graphite
mv carbon-0.9.10 carbon
mv whisper-0.9.10 whisper
rm graphite-web-0.9.10.tar.gz
rm carbon-0.9.10.tar.gz
rm whisper-0.9.10.tar.gz

easy_install django-tagging

easy_install zope.interface
 
easy_install twisted
  
easy_install txamqp

pushd whisper
python setup.py install
popd

pushd carbon
python setup.py install
popd 

pushd /opt/graphite/conf
sudo cp carbon.conf.example carbon.conf
sudo cp storage-schemas.conf.example storage-schemas.conf
echo "
[stats]
priority = 110
pattern = .*
retentions = 10:2160,60:10080,600:262974
" > storage-schemas.conf
popd

pushd graphite
python check-dependencies.py
python setup.py install
popd

pushd graphite/examples
cp example-graphite-vhost.conf /etc/apache2/sites-available/default
cp /opt/graphite/conf/graphite.wsgi.example /opt/graphite/conf/graphite.wsgi
mkdir /etc/httpd
mkdir /etc/httpd/wsgi
popd

sed -i 's@^WSGISocketPrefix.*$@WSGISocketPrefix /etc/httpd/wsgi@g' /etc/apache2/sites-available/default
service apache2 restart

pushd /opt/graphite/webapp/graphite/
echo '[{"pk": 14, "model": "contenttypes.contenttype", "fields": {"model": "contenttype", "name": "content type", "app_label": "contenttypes"}}, {"pk": 6, "model": "contenttypes.contenttype", "fields": {"model": "dashboard", "name": "dashboard", "app_label": "dashboard"}}, {"pk": 7, "model": "contenttypes.contenttype", "fields": {"model": "event", "name": "event", "app_label": "events"}}, {"pk": 9, "model": "contenttypes.contenttype", "fields": {"model": "group", "name": "group", "app_label": "auth"}}, {"pk": 13, "model": "contenttypes.contenttype", "fields": {"model": "logentry", "name": "log entry", "app_label": "admin"}}, {"pk": 11, "model": "contenttypes.contenttype", "fields": {"model": "message", "name": "message", "app_label": "auth"}}, {"pk": 5, "model": "contenttypes.contenttype", "fields": {"model": "mygraph", "name": "my graph", "app_label": "account"}}, {"pk": 8, "model": "contenttypes.contenttype", "fields": {"model": "permission", "name": "permission", "app_label": "auth"}}, {"pk": 1, "model": "contenttypes.contenttype", "fields": {"model": "profile", "name": "profile", "app_label": "account"}}, {"pk": 12, "model": "contenttypes.contenttype", "fields": {"model": "session", "name": "session", "app_label": "sessions"}}, {"pk": 15, "model": "contenttypes.contenttype", "fields": {"model": "tag", "name": "tag", "app_label": "tagging"}}, {"pk": 16, "model": "contenttypes.contenttype", "fields": {"model": "taggeditem", "name": "tagged item", "app_label": "tagging"}}, {"pk": 10, "model": "contenttypes.contenttype", "fields": {"model": "user", "name": "user", "app_label": "auth"}}, {"pk": 2, "model": "contenttypes.contenttype", "fields": {"model": "variable", "name": "variable", "app_label": "account"}}, {"pk": 3, "model": "contenttypes.contenttype", "fields": {"model": "view", "name": "view", "app_label": "account"}}, {"pk": 4, "model": "contenttypes.contenttype", "fields": {"model": "window", "name": "window", "app_label": "account"}}, {"pk": 13, "model": "auth.permission", "fields": {"codename": "add_mygraph", "name": "Can add my graph", "content_type": 5}}, {"pk": 14, "model": "auth.permission", "fields": {"codename": "change_mygraph", "name": "Can change my graph", "content_type": 5}}, {"pk": 15, "model": "auth.permission", "fields": {"codename": "delete_mygraph", "name": "Can delete my graph", "content_type": 5}}, {"pk": 1, "model": "auth.permission", "fields": {"codename": "add_profile", "name": "Can add profile", "content_type": 1}}, {"pk": 2, "model": "auth.permission", "fields": {"codename": "change_profile", "name": "Can change profile", "content_type": 1}}, {"pk": 3, "model": "auth.permission", "fields": {"codename": "delete_profile", "name": "Can delete profile", "content_type": 1}}, {"pk": 4, "model": "auth.permission", "fields": {"codename": "add_variable", "name": "Can add variable", "content_type": 2}}, {"pk": 5, "model": "auth.permission", "fields": {"codename": "change_variable", "name": "Can change variable", "content_type": 2}}, {"pk": 6, "model": "auth.permission", "fields": {"codename": "delete_variable", "name": "Can delete variable", "content_type": 2}}, {"pk": 7, "model": "auth.permission", "fields": {"codename": "add_view", "name": "Can add view", "content_type": 3}}, {"pk": 8, "model": "auth.permission", "fields": {"codename": "change_view", "name": "Can change view", "content_type": 3}}, {"pk": 9, "model": "auth.permission", "fields": {"codename": "delete_view", "name": "Can delete view", "content_type": 3}}, {"pk": 10, "model": "auth.permission", "fields": {"codename": "add_window", "name": "Can add window", "content_type": 4}}, {"pk": 11, "model": "auth.permission", "fields": {"codename": "change_window", "name": "Can change window", "content_type": 4}}, {"pk": 12, "model": "auth.permission", "fields": {"codename": "delete_window", "name": "Can delete window", "content_type": 4}}, {"pk": 37, "model": "auth.permission", "fields": {"codename": "add_logentry", "name": "Can add log entry", "content_type": 13}}, {"pk": 38, "model": "auth.permission", "fields": {"codename": "change_logentry", "name": "Can change log entry", "content_type": 13}}, {"pk": 39, "model": "auth.permission", "fields": {"codename": "delete_logentry", "name": "Can delete log entry", "content_type": 13}}, {"pk": 25, "model": "auth.permission", "fields": {"codename": "add_group", "name": "Can add group", "content_type": 9}}, {"pk": 26, "model": "auth.permission", "fields": {"codename": "change_group", "name": "Can change group", "content_type": 9}}, {"pk": 27, "model": "auth.permission", "fields": {"codename": "delete_group", "name": "Can delete group", "content_type": 9}}, {"pk": 31, "model": "auth.permission", "fields": {"codename": "add_message", "name": "Can add message", "content_type": 11}}, {"pk": 32, "model": "auth.permission", "fields": {"codename": "change_message", "name": "Can change message", "content_type": 11}}, {"pk": 33, "model": "auth.permission", "fields": {"codename": "delete_message", "name": "Can delete message", "content_type": 11}}, {"pk": 22, "model": "auth.permission", "fields": {"codename": "add_permission", "name": "Can add permission", "content_type": 8}}, {"pk": 23, "model": "auth.permission", "fields": {"codename": "change_permission", "name": "Can change permission", "content_type": 8}}, {"pk": 24, "model": "auth.permission", "fields": {"codename": "delete_permission", "name": "Can delete permission", "content_type": 8}}, {"pk": 28, "model": "auth.permission", "fields": {"codename": "add_user", "name": "Can add user", "content_type": 10}}, {"pk": 29, "model": "auth.permission", "fields": {"codename": "change_user", "name": "Can change user", "content_type": 10}}, {"pk": 30, "model": "auth.permission", "fields": {"codename": "delete_user", "name": "Can delete user", "content_type": 10}}, {"pk": 40, "model": "auth.permission", "fields": {"codename": "add_contenttype", "name": "Can add content type", "content_type": 14}}, {"pk": 41, "model": "auth.permission", "fields": {"codename": "change_contenttype", "name": "Can change content type", "content_type": 14}}, {"pk": 42, "model": "auth.permission", "fields": {"codename": "delete_contenttype", "name": "Can delete content type", "content_type": 14}}, {"pk": 16, "model": "auth.permission", "fields": {"codename": "add_dashboard", "name": "Can add dashboard", "content_type": 6}}, {"pk": 17, "model": "auth.permission", "fields": {"codename": "change_dashboard", "name": "Can change dashboard", "content_type": 6}}, {"pk": 18, "model": "auth.permission", "fields": {"codename": "delete_dashboard", "name": "Can delete dashboard", "content_type": 6}}, {"pk": 19, "model": "auth.permission", "fields": {"codename": "add_event", "name": "Can add event", "content_type": 7}}, {"pk": 20, "model": "auth.permission", "fields": {"codename": "change_event", "name": "Can change event", "content_type": 7}}, {"pk": 21, "model": "auth.permission", "fields": {"codename": "delete_event", "name": "Can delete event", "content_type": 7}}, {"pk": 34, "model": "auth.permission", "fields": {"codename": "add_session", "name": "Can add session", "content_type": 12}}, {"pk": 35, "model": "auth.permission", "fields": {"codename": "change_session", "name": "Can change session", "content_type": 12}}, {"pk": 36, "model": "auth.permission", "fields": {"codename": "delete_session", "name": "Can delete session", "content_type": 12}}, {"pk": 43, "model": "auth.permission", "fields": {"codename": "add_tag", "name": "Can add tag", "content_type": 15}}, {"pk": 44, "model": "auth.permission", "fields": {"codename": "change_tag", "name": "Can change tag", "content_type": 15}}, {"pk": 45, "model": "auth.permission", "fields": {"codename": "delete_tag", "name": "Can delete tag", "content_type": 15}}, {"pk": 46, "model": "auth.permission", "fields": {"codename": "add_taggeditem", "name": "Can add tagged item", "content_type": 16}}, {"pk": 47, "model": "auth.permission", "fields": {"codename": "change_taggeditem", "name": "Can change tagged item", "content_type": 16}}, {"pk": 48, "model": "auth.permission", "fields": {"codename": "delete_taggeditem", "name": "Can delete tagged item", "content_type": 16}}, {"pk": 1, "model": "auth.user", "fields": {"username": "root", "first_name": "", "last_name": "", "is_active": true, "is_superuser": true, "is_staff": true, "last_login": "2013-11-09 13:09:47", "groups": [], "user_permissions": [], "password": "sha1$0187c$85f821799d37bb7c36a0c3aeb80cc98c014d3374", "email": "root@localhost.local", "date_joined": "2013-11-09 13:09:47"}}]' >> ./initial_data.json
echo "Getting ready to do python manage.py syncdb --noinput"
python manage.py syncdb --noinput
popd

chown -R www-data:www-data /opt/graphite/storage/
/etc/init.d/apache2 restart

pushd /opt/graphite/webapp/graphite
cp local_settings.py.example local_settings.py
popd

pushd /opt/graphite/
./bin/carbon-cache.py start
popd



ENDSCRIPT
#########################

Vagrant.configure("2") do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://dl.dropbox.com/u/1537815/precise64.box"
  # This assume VirtualBox and has the tools installed. 
  #
  # For more boxes, check 
  # http://www.vagrantbox.es/
  # Many thanks to Gareth Rushgrove (http://www.morethanseven.net/)

  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.hostname = "graphite"
  config.vm.provision :shell, inline: $initScript

  config.vm.provision :puppet do |puppet|
  	puppet.manifests_path = "puppet/manifests"
	puppet.module_path = "puppet/modules"
	puppet.manifest_file = "init.pp"
  end

end
